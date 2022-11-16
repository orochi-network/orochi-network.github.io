### Circuit Specification
Let \\(\mathbb{F}\\) be a finite field. In this section, we describe the arithmetic circuit whose operations are over \\(\mathbb{F}\\). 

Let \\(\ell_{\mathsf{in}} \in \mathbb{N}\\) be the number of input wires of the circuit \\(\mathcal{C}\\). Assume that \\(\mathcal{C}\\) has exactly $n$ gates. Each gate takes at most \\(2\\) wires as inputs and returns \\(1\\) output wires. In particular,
- Addition and multiplications gates takes \\(2\\) inputs and return \\(1\\) output.
- Gates of additions and multiplications with constants take \\(1\\) input and return \\(1\\) output.

