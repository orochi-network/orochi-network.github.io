# PlonK
In this chapter, we will present the construction of {{#cite GWC19}}, i.e., permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge.

PlonK is a succinct non-interactive zero-knowledge argument (SNARK) system that proves the correct execution of a program, i.e., in this case, an arithmetic circuit with only addition \\((+)\\) and multiplication \\((\cdot)\\) operations.

As an overview of the construction, we separate it into \\(2\\) parts. First, we transform the arithmetic circuit into a set of constraints, called arithmetization and represented under some form of polynomials. Then, by applying some proof technique, it compiles the arithmetization into the proof.

**Author(s):** [khaihanhtang](https://github.com/khaihanhtang)