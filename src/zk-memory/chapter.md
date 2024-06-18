# zkMemory
In this chapter we give a brief overview to zkMemory, including its components and functionalities.

zkMemory is a module built by Orochi Network which allows a prover to prove the consistency when reading and writing values in the memory. The problem can be stated as follows:

**Consider a memory \\(M\\). Then for any cell of \\(M\\), during the course of time, whenever we read from the cell, the read value must be equal to the last time it was written in the same cell.**

Proving memory consistency is an important sub-component for handling the correctness RAM program, which can be used to construct zkVMs. Therefore, zkMemory could serve as a powerful library which handle the memory part in zkVMs and helps developers build custom zkVMs tailored to specific needs within the Web3 landscape. We divide this chapter into \\(3\\)  major parts below:

- In [Execution Trace](./execution-trace/execution-trace.md), we describe **execution trace**, which are used to prove memory consistency from a memory \\(M\\) and a computation process of \\(N\\) steps.
  
- In [Commitment Schemes](./commitment/commitment.md), we describe several commitment schemes for committing the execution trace that we support.
  
- In [Memory Consistency Constraints](./constraints/constraints.md), we show how to prove the consistency of memory given the execution trace and how do we integrate Halo2 for the constraint.
