# Transforming to PLONKish Arithmetization
To setup the above sequence of computations into PLONKish arithmetization, we specify a table to contain all possible variables appeared during the computing the arithmetic circuit fir \\(u^2 + 3uv + v + 5\\). Then, for each row, we setup a few gates, or equivalently gate constraints, that applies to the row. Specificially, we define the following tuple of columns
$$
    (\mathsf{advice} _0, \mathsf{advice} _1, \mathsf{advice} _2, \mathsf{constant}, \mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}})
$$
where
- \\(\mathsf{advice}_0, \mathsf{advice}_1, \mathsf{advice}_2\\) are columns containing private inputs, i.e., values belonging to these columns are hidden to any verifier,
- \\(\mathsf{constant}\\) is the column containing public constants appearing during the computation,
- \\(\mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}}\\) contain public selectors.

We now explain the intuition to the above setting of columns. To do this, we need to transform the sequence of computations, from step 3, in [A Simple Arithmetic Circuit](./simple_arithmetic_circuit.md) into \\(2\\) parts, namely, gate constraints (or gate identities) and wire constraints (or wire identities). In particular, we transform
$$
    \begin{cases}   
        t_1 = u^2,\\\\
        t_2 = u v,\\\\
        t_3 = t_2 \cdot 3 = 3uv,\\\\
        t_4 = t_1 + t_3 = u^2 + 3uv,\\\\
        t_5 = t_4 + v = u^2 + 3uv + v,\\\\
        t_6 = t_5 + 5 = u^2 + 3uv + v + 5
    \end{cases}
$$
 to gate constraints
$$
    \begin{cases}
        x_{c_1} = x_{a_1} x_{b_1},\\\\
        x_{c_2} = x_{a_2}  x_{b_2},\\\\
        x_{c_3} = x_{a_3} \cdot 3,\\\\
        x_{c_4} = x_{a_4} + x_{b_4},\\\\
        x_{c_5} = x_{a_5} + x_{b_5},\\\\
        x_{c_6} = x_{a_6} + 5
    \end{cases}
$$
and wire constraints
$$
    \begin{cases}
        u = x_{a_1} = x_{a_2} = x_{b_1},\\\\
        v = x_{b_2} = x_{b_5},\\\\
        t_1 = x_{a_4} = x_{c_1},\\\\
        t_2 = x_{a_3} = x_{c_2},\\\\
        t_3 = x_{b_4} = x_{c_3},\\\\
        t_4 = x_{a_5} = x_{c_4},\\\\
        t_5 = x_{a_6} = x_{c_5},\\\\
        t_6 = x_{c_6}.
    \end{cases}
$$

Hence, for <span style="color:red">continue writing here<span>