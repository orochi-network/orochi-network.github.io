# Technical Overview

From the syntax of polynomial commitment scheme presented in [Polynomial Commitment Scheme - Syntax](./../pcs_definition/syntax.md), a realization, or equivalently, a construction, can be separated into \\(2\\) components:
- Commiting polynomial includes the algorithms \\(\mathsf{Commit}\\) and \\(\mathsf{VerifyPoly}\\).
- Evaluating polynomial includes the algorithms \\(\mathsf{CreateWitness}\\) and \\(\mathsf{VerifyEval}\\).

Based on those components, we present the high level idea of \\(2\\) constructions, namely, conditional and unconditional, of polynomial commitment scheme separated into \\(3\\) little parts. The first and second parts, to be presented in [Commitment to Polynomial Without Hiding Property](./commitment_without_hiding.md) and [Correct Evaluation from the Commitment](./correct_evaluation_from_commitment.md), respectively, focus on constructing the realization of conditional version. And, in the third part, to be presented in [Dealing with Hiding](./dealing_with_hiding.md), regarding condition and unconditional hidings, we discuss the modification of conditional version to achieve the unconditional one.
