# zkMemory
In this chapter we give a brief overview to zkMemory, including its components and functionalities.

zkMemory is a module built by Orochi Network which allows a prover to prove the consistency when reading and writing values in the memory. Proving memory consistency is an important sub-component for handling the correctness RAM program, which can be used to construct zkVMs. Therefore, zkMemory could serve as a powerful library which handle the memory part in zkVMs and helps developers build custom zkVMs tailored to specific needs within the Web3 landscape. We divide this chapter into \\(3\\)  major parts below:

- In , we describe **execution trace**, which are used to prove memory consistency from a memory \\(M\\) and a computation process of \\(N\\) steps.
- In , we describe several commitment schemes for committing the execution trace that we support.
- In , we show how to prove the consistency of memory given the execution trace and how do we integrate Halo2 for the constraint.
