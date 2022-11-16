### Breaking Circuit
To break the circuit into gates with wires separated, namely, no wire involves to \\(2\\) or more gates, we use a set of labels \\(\mathcal{I} = \\{a_1, \dots, a_n, b_1, \dots, b_n, c_1, \dots, c_n\\}\\) to denote the wire label of each gate. Let \\(x : \mathcal{I} \to \mathbb{F}\\) be the function mapping wires to their respective wire values. Hence, \\(x(id)\\) represents the value at wire \\(id \in \mathcal{I}\\) For simplicity, we write \\(x_{id}\\), in place of \\(x(id)\\), for any \\(id \in \mathcal{I}\\).

Specifically, for each \\(i \in \\{1, \dots, n\\}\\), we denote by
- \\(x_{c_i} = x_{a_i}\circ x_{b_i}\\) to denote the computation where \\(\circ\\) is either addition or multiplication.
- \\(x_{c_i} = x_{a_i}\circ c\\) to denote the computation where \\(\circ\\) is either addition or multiplication with constant and \\(c\\) is the constant.

> At this point, we may be curious what value \\(x_{b_i}\\) takes if \\(\circ\\) is operation with constant. In fact, in this case x_{b_i} can be seen as garbage and can take any value from \\(\mathbb{F}\\). This value will affect neither security nor correctness of the system since the constraints in PlonK's arithmetization guarantee that such a compromise will not happen.

Let's take a look at the following example, taken from [Circuit Specification](./subsection_circuit_specification.md). 
> We have \\(f(u, v) = u^2 + 3uv + v + 5\\) and the constraints below.
> $$
    \begin{cases}
        z_1 = u \cdot u &\text{(multiplication)},\\\\
        z_2 = u \cdot v &\text{(multiplication)},\\\\
        z_3 = z_2 \cdot 3 &\text{(multiplication with constant)},\\\\
        z_4 = z_1 + z_3 &\text{(addition)},\\\\
        z_5 = z_4 + v &\text{(addition)},\\\\
        z_6 = z_5 + 5 &\text{(addition with constant)}.
    \end{cases}
> $$
> By breaking circuit, we have the following constraints with respect to the above format, namely, using \\(\mathcal{I} = \\{a_1, \dots, a_6, b_1, \dots, b_6, c_1, \dots, c_6\\}\\), where \\(n = 6\\), and \\(x : \mathcal{I}\to \mathbb{F}\\).
> $$
    \begin{cases}
        x_{c_1} = x_{a_1} \cdot x_{b_1},\\\\
        x_{c_2} = x_{a_2} \cdot x_{b_2},\\\\
        x_{c_3} = x_{a_3} \cdot 3,\\\\
        x_{c_4} = x_{a_4} + x_{b_4},\\\\
        x_{c_5} = x_{a_5} + x_{b_5}, \\\\
        x_{c_6} = x_{a_6} + 5
    \end{cases}\text{ where }
    \begin{cases}
        u = x_{a_1} = x_{a_2} = x_{b_1},\\\\
        v = x_{b_2} = x_{b_5},\\\\
        z_1 = x_{a_4} = x_{c_1},\\\\
        z_2 = x_{a_3} = x_{c_2},\\\\
        z_3 = x_{b_4} = x_{c_3},\\\\
        z_4 = x_{a_5} = x_{c_4},\\\\
        z_5 = x_{a_6} = x_{c_5},\\\\
        z_6 = x_{c_6}.\\\
    \end{cases}
> $$
> Notice that \\(v_{b_3}\\) and \\(v_{b_6}\\) are dismissed. These values can be any elements from \mathbb{F} and do not have any effect to the arithmetization.

For equations in the system guaranteeing the correct computation of the operation, we call them *gate constraints*.

For equations in the system guaranteeing the equalities, or consistencies, among wires, we call them *copy constraints*.

We first discuss the transformation of gate constraints into a common unified form with publicly determined selectors in [Gate Constraints](./subsection_gate_constraints.md). Then, we discuss the method for guaranteeing copy constraints in [Copy Constraints](./subsection_copy_constraints.md).