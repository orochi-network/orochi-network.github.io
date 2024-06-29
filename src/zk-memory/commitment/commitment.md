## Commitment Schemes

The first step in zkMemory is to commit the memory, then later we prove the correctness of the trace where its commitment serves as the public instance to prevent prover from cheating in the computation process. Later, we employ a ZKP protocol to prove that the execution trace is correct given its commitment. Currently, our zkMemory implementation supports 3 commitment schemes: KZG, Merkle Tree and Verkle Tree. We now give a high overview of these schemes.

### KZG Commitment Scheme

KZG is the first commitment scheme we intend to support, which is widely used in various ZKP systems such as Halo2. We have already presented an overview of KZG [here](./../../kzg-polynomial-commitment-scheme/chapter.md), therefore we refer the readers unfamiliar with KZG to the link. Let \\(\mathsf{F}\\) be a finite field, and let \\(\omega\\) be a root of \\(4\\)-th unity. To commit each trace in the trace, we first create a polynomial \\(p(x)\\) such that:

$$p(1)=addr,~p(\omega)=time,~p(\omega^2)=op,~p(\omega^3)=val$$

After creating the polynomial \\(p(x)\\), we use the KZG commitment algorithm to commit it which we denote \\(\mathsf{com}(p(x))\\), and the whole trace \\(\mathsf{com}(p_i(x))_{i=1}^n\\) is the commitment of the whole execution trace. Later, if needed, we can open any trace in the trace using the techniques in [this](https://dankradfeist.de/ethereum/2021/06/18/pcs-multiproofs.html), which provides constant opening size and verification time.

### Merkle Tree Commitment Scheme

Merkle Tree is the second commitment scheme we would support to commit the memory. At a high level, each leaf of a tree is assigned a value, and the value of the parent is equal to the hash of its children. When opening a leaf node, we only need to reveal all the values in the path from the leaf to the root as well as the sibling nodes in the path. The collision resistance property of cryptographic hash functions ensures that the prover cannot produce an invalid opening.

There are two methods for committing memory using Merkle tree. The first method is to commit to the trace, similar like what we did using KZG commitment. However there is a second method, which we commit to the __memory cells__ instead of the trace, where we assign each leaf node to be a memory cell. In each step, we update the commitment from \\(C\\) to \\(C'\\) and provide a Verkle proof that \\(C\\) was indeed updated to \\(C'\\). This ``proof providing'' process can be built with a circuit to hide everything except the instances, which are \\(C\\) and \\(C'\\). The second method will be used to support memory consistency from IVC-based constructions like the Nova proof system, as currently we are additionally integrating Nova to support memory consistency in our zkMemory module.

### Verkle Tree Commitment Scheme

Verkle tree is very similar to Merkle tree, However, there are several benefits of using Verkle tree compared to Merkle tree as follows:

- The number of children of each parent node in Verkle tree is much greater, making the tree have a much lower height when committing the same data.
- In addition, the number of children can be configured to one's desire, making the tree more flexible when using proof systems than Merkle Tree.

Finally, to commit the memory with Verkle tree, we also do the same as Merkle tree, except that we now employ KZG commitment scheme to commit to the children, which enables constant proof size and verification time, which is also another huge benefit compared to Merkle tree according to [this](https://dankradfeist.de/ethereum/2021/06/18/pcs-multiproofs.html).
