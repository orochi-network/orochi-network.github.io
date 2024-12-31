# RAMenPaSTA: Parallelizable Scalable Transparent Arguments of Knowledge for RAM Programs

In this chapter, we give a detailed description of [RAMenPaSTA](https://eprint.iacr.org/2024/336) - a novel research work sponsored by Orochi Network which introduces the first construction of a parallelizable folding scheme, resulting in the first construction of a parallelizable and verifiable RAM programs (pvRAM).

Incremental Verifiable Computation (IVC) {{#cite Pa08}} is a cryptographic primitive that enables producing proofs of correct execution of sequential computations such that a verifier can efficiently verify the correct execution of any prefix of the computation, in which each step the computation can receive some private auxiliary prover-owned input besides the output of the previous step.

Due to the sequential nature of the computation, the IVC schemes have a vast number of applications, such as scaling blockchains via Layer-2 solutions, verifiable delay functions {{#cite BBBF18}}, verifiable replicated state-machines (e.g., TinyRAM {{#cite BCGTV13}}), etc.

To realize IVC, the following approaches have been explored:

- **Proof-Carrying-Data (PCD):** Where mutually distrusted parties can perform pipelined efficiently verifiable private computation {{#cite CT10}}. There are two approaches to achieve PCD:
  - **zk-SNARKs:** Verify the correctness of intermediate steps using explicit SNARK proofs {{#cite BCIOP13}} {{#cite GGPR13}}.
  - **Accumulation Schemes:** Accumulate verification of arguments at intermediate steps and verify them at the end {{#cite bcms20}} {{#cite BCLMS21}}.

- **Folding Schemes (FS):** This approach accumulates the instances via the folding process and defers the verification of this folded instance until the end of the computation, which significantly reduces the prover cost {{#cite KST22}}. Recent advancements include {{#cite KS22}} {{#cite KS23}} {{#cite ZGGX23}} {{#cite LXZS24}}.

The above works on Folding Schemes instantiate the Random Oracle Model (ROM) using a cryptographic hash function. The usage of ROs is at two places:

- At each step, the RO is invoked to do the verification for proof that the output of the previous step matches the input of the next step. Such connections between steps are formed recursively across steps, causing non-constant depth recursion heuristics.
- At the end of the computation, the final proof is "wrapped" in a SNARK for succinctness, which also requires RO invocation.

Since the RO cannot be represented by a circuit, the folding/accumulation-based IVC fails to obtain provable security. If we aim for a succinct verifier, the second usage of the RO is unavoidable. However, the recursion heuristic in the input/output connections is not the case.

To achieve provable security while avoiding the use of recursive heuristic RO invocations for the IVC scheme, {{#cite TPN24}} introduces a new variant of folding scheme called **Conditional Folding (CF)** scheme, which accumulates and defers verification of connections until the end, thereby avoiding recursive RO invocations (recursion heuristic), creating a **hybrid folding-accumulation scheme**. This scheme supports parallelizable incremental computations with memory and has \\( O(|W| \\log N) \\) prover time complexity, where \\( |W| \\) is the size of witness in an instance-witness pair and \\( N \\) is the number of steps in the computation. It also supports incomplete binary folding structures and perfect hiding of instruction indices in each step. The CF scheme enables the construction of a **parallelizable and verifiable RAM programs (pvRAM)** argument with proven security in the generic group model, that is zero-knowledge, requires no trusted setup, and hides program counters/instructions. This construction of pvRAM is named **RAMenPaSTA** in the paper.

# Specification

The structure of this chapter is as follows:

- [Generic Relaxed R1CS](./ramenpasta-construction/generic-relaxed-r1cs.md): This section introduces a new variant of relaxed R1CS, which is compatible with parallelizable folding schemes.
- [Conditional Folding Scheme for Generic Relaxed R1CS](./ramenpasta-construction/conditional-folding-scheme.md): This part describes the construction of the folding scheme for generic relaxed R1CS.
- [pvRAM from Conditional Folding Scheme](./ramenpasta-construction/pvRAM-from-conditional-folding-scheme.md): This part describes the construction of a parallelizable and verifiable RAM program (pvRAM) from the folding scheme.
