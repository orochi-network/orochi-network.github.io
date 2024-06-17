## Commitment

The first step in zkMemory is to commit the execution trace, then later we prove the correctness of the trace where its commitment serves as  the public instance to prevent prover from cheating in the computation process. Later, we employ a ZKP protocol to prove that the execution trace is correct given its commitment. Currently, our implementation supports 3 commitment schemes: KZG, Merkle Tree and Verkle Tree. We now give a high overview of these schemes.

### KZG Commitment Scheme

KZG is the first commitment scheme we intend to support. It is widely used in various ZKP systems such as Halo2.

### Merkle Tree Commitment Scheme

### Verkle Tree Commitment Scheme