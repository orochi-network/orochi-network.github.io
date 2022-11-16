### Circuit Specification
Let \\(\mathbb{F}\\) be a finite field. In this section, we describe the arithmetic circuit whose operations are over \\(\mathbb{F}\\). 

Let \\(\ell_{\mathsf{in}} \in \mathbb{N}\\) be the number of input wires of the circuit \\(\mathcal{C}\\). Assume that \\(\mathcal{C}\\) has exactly \\(n\\) gates. Each gate takes at most \\(2\\) wires as inputs and returns \\(1\\) output wires. In particular,
- Addition and multiplications gates takes \\(2\\) inputs and return \\(1\\) output.
- Gates of additions and multiplications with constants take \\(1\\) input and return \\(1\\) output.

Let's take a look at the following example.
> Assume that \\(f(u, v) = u^2 + 3uv + v + 5\\). Then the sequence are arranged in the following constraints, wrapped as below.
> $$
    \begin{cases}
        z_1 = u \cdot u &\text{(multiplication)},\\\\
        z_2 = u \cdot v &\text{(multiplication)},\\\\
        z_3 = z_2 \cdot 3 &\text{(multiplication with constant)},\\\\
        z_4 = z_1 + z_3 &\text{(addition)},\\\\
        z_5 = z_4 + v &\text{(addition)},\\\\
        z_6 = z_5 + 5 &\text{(addition with constant)}.
    \end{cases}
$$
> The input size is \\(\ell_{\mathsf{in}} = 2\\) for variables \\(u, v\\) and the output is \\(z_6\\).