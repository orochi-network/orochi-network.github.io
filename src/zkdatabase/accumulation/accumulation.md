# Accumluation

## The problem statement
Suppose we have a zkApp that updates its state with each user interaction. The updated state relies on both the previous state and the inputs from the interaction – a scenario likely common to many zkApps. The straightforward approach is to update the on-chain state during every interaction. However, to derive the new state, we must refer to the existing state. To validate that we're genuinely referencing the on-chain state, a precondition is set on the present state. Here's the catch: if multiple users unknowingly send their transactions within the same block, they'll establish the same precondition based on the current state, and concurrently attempt to update it. As a result, every transaction following the initial one becomes obsolete due to a stale precondition and gets declined.

## Accumulation and Concurrency in Mina

### Accumulation scheme
An accumulation scheme is a scheme that allows the prover to combine several proofs into a single proof, which can be verified more efficiently compared to verifying each proof separately. This scheme is critical in scaling blockchain systems and other similar systems, which involve numerous transactions and hence, multiple proofs.

[Mina's Proof Systems](https://o1-labs.github.io/proof-systems/pickles/accumulation.html) - offers deep insights into the utilization and implementation of accumulation schemes.

### Block production

In the Mina blockchain, transactions are grouped into blocks and added to the blockchain in a sequential manner. The blocks are created by block producers (similar to miners in other blockchain networks), and the block producers are responsible for processing the transactions in the blocks they produce.

Now, when multiple transactions are sent to the same smart contract, and possibly calling the same function within a short time frame, they would be processed one after the other, in the order they are included in the block by the block producer. 

[Mina Block Production](https://docs.staketab.com/academy/mina/mina-block-production) - a comprehensive guide to understanding the nuances of block production within the Mina network.

### Handling Concurrent Calls to the Same Function

In the context of simultaneous calls to the same function in a smart contract:

- Atomicity: Each transaction is processed atomically. It will see a consistent state and produce a consistent state update.

- Isolation: Transactions are isolated from each other until they are included in the block, at which point the state changes become visible to subsequent transactions.

- Concurrency Issues: If two transactions are modifying the same piece of data, they will do so in the order determined by their position in the block, which prevents conflicts but can potentially lead to situations like __front-running__.

## Accumulation in o1js

Actions, similar to events, are public data pieces shared with a zkApp transaction. What sets actions apart is their enhanced capability: they enable processing of past actions within a smart contract. This functionality is rooted in a commitment to the historical record of dispatched actions stored in every account, known as the **actionState**. This ensures verification that the processed actions align with those dispatched to the identical smart contract.

### Practical Accumulation: Managing Accumulation in Code

A code-based approach to handle accumulation using thresholds:

```ts
@state root = State<Field>();
@state actionsHash = State<Field>();
@state userCount = State<Field>();
reducer = Reducer({ actionType: MerkleUpdate})

@method
deposit(user: PublicKey, amount: Field, witness: Witness) {
    this.userCount.set(this.userCount.get() + 1);
	this.root.assertEquals(this.root.get());
	this.root.assertEquals(witness.computeRoot(amount));
	user.send(this.account, amount);
	this.reducer.dispatch({ witness: witness, newLeaf: amount })

    if (this.userCount.get() >= TRANSACTION_THRESHOLD) {
        // if we reach a certain treshold, we process all accumulated data
        let root = this.root.get();
        let actionsHash = this.actionsHash.get();
        
        let { state: newRoot, actionsHash: newActionsHash } = this.reducer.reduce(
            this.reducer.getActions(actionsHash),
            MerkleUpdate,
            (state: Field, action: MerkleUpdate) => {
                return action.witness.computeRoot(action.newLeaf);
            }
        );
        
        this.root.set(newRoot);
        this.actionsHash.set(newActionsHash);
        this.userCount.set(0);
    }
}
```

### API
```ts
reducer = Reducer({actionType: MyAction})
```
**reducer.dispatch(action: Field)**
- Description:
    Dispatches an Action. Similar to normal Events, Actions can be stored by archive nodes and later reduced within a SmartContract method to change the state of the contract accordingly

- Parameters:
    - **action (MyAction)**: Action to be stored.

**reducer.getActions(currentActionState: Field, endActionState:: Field)**
- Description:
    Fetches the list of previously emitted **Actions**. Supposed to be used in the circuit.

- Parameters:
    - **currentActionState (Field)**: The hash of already processed actions.
    - **endActionState (Field)**: Calculated state of non-processed actions. Can be either `account.actionState` or be calculated with `Actions.updateSequenceState`.

- Returns: Returns the dispatched actions within the designated range. 

**reducer.fetchActions(currentActionState: Field, endActionState:: Field)**
- Description:
    Fetches the list of previously emitted **Actions**. Supposed to be used out of circuit.

- Parameters:
    - **currentActionState (Field)**: The hash of already processed actions.
    - **endActionState (Field)**: Calculated state of non-processed actions. Can be either `account.actionState` or be calculated with `Actions.updateSequenceState` .

- Returns: Returns the dispatched actions within the designated range. 

**reducer.reduce(currentActionState: Field, endActionState:: Field)**
- Description:
    Reduces a list of Actions, similar to `Array.reduce()`.

- Parameters:
    - **actions (Action[][])**:  A list of sequences of pending actions. The maximum number of columns in a row is capped at 5. (P.S. A row is populated when multiple dispatch calls are made within a single @method.)
    - **stateType (Provable<State>)**: Specifies the type of the state
    - **initial**: An object comprising:
        - **state (State)**: The current state.
        - **actionState (Field)**: A pointer to actions in the historical record.
    - **maxTransactionsWithActions (optional, number)**: Defines the upper limit on the number of actions to process simultaneously. The default value is 32. This constraint exists because o1js cannot handle dynamic lists of variable sizes. The max value is limited by the circuit size.
    - **skipActionStatePrecondition (optional, boolean)**: Skip the precondition assertion on the account. Each account has `account.actionState` which is the hash of all dispatched items. So, after reducing we check if all processed is equal to the all reduced once.

- Returns:
    - **state** (State): Represents the updated state of the zk app, e.g., the concluding root.
    - **actionState** (Field): Indicates the position of the state. It is essentially the hash of the actions that have been processed.


### Manual calculation of the pointer to the actions history
```ts
    let actionHashs = AccountUpdate.Actions.hash(pendingActions);
    let newState = AccountUpdate.Actions.updateSequenceState(currentActionState, actionHashs);
```

### Commutative
Actions are processed without a set order. To avoid race conditions in the zkApp, actions should be interchangeable against any state. For any two actions, a1 and a2, with a state `s`, the result of `s * a1 * a2` should be the same as `s * a2 * a1`.

### Action Location
All actions are stored in archive nodes.

# Resources
- https://github.com/o1-labs/o1js/issues/265
- https://github.com/o1-labs/snarkyjs/issues/659
- https://docs.minaprotocol.com/zkapps/o1js/actions-and-reducer