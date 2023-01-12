# Transforming to PLONKish Arithmetization
To setup the above sequence of computations, in [A Simple Arithmetic Circuit](./simple_arithmetic_circuit.md), into PLONKish arithmetization, we specify a table to contain all possible variables appeared during the computing the arithmetic circuit for \\(f(u,v) = u^2 + 3uv + v + 5\\). Then, for each row, we setup a the set gates, or equivalently gate constraints, that applies to the row. 

## Specifying columns
We define the following tuple of columns
$$
    (\mathsf{advice} _0, \mathsf{advice} _1, \mathsf{advice} _2, \mathsf{constant}, \mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}})
$$
where
- \\(\mathsf{advice}_0, \mathsf{advice}_1, \mathsf{advice}_2\\) are columns containing private inputs, i.e., values belonging to these columns are hidden to any verifier,
- \\(\mathsf{constant}\\) is the column containing public constants appearing during the computation,
- \\(\mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}}\\) contain public selectors corresponding to addition, multiplication, addition with constant, multiplication with constant, respectively, gates.

## Transforming to Constraints and Meaning of Columns
We now explain the intuition to the above setting of columns. To do this, we need to transform the sequence of computations in [A Simple Arithmetic Circuit](./simple_arithmetic_circuit.md) into \\(2\\) parts, namely, gate constraints (or gate identities) and wire constraints (or wire identities). In particular, we transform
$$
    \begin{align}   
        t^{(1)} &= u^2,  &t^{(3)} &= t^{(2)} \cdot 3 = 3uv, & t^{(5)} &= t^{(4)} + v = u^2 + 3uv + v,\\\\
        t^{(2)} &= u v, & t^{(4)} &= t^{(1)} + t^{(3)} = u^2 + 3uv, &t^{(6)} &= t^{(5)} + 5 = u^2 + 3uv + v + 5
    \end{align}
$$
to gate constraints

$$
    \begin{align}
        x ^{(1)} _{c} &= x ^{(1)} _{a} \cdot x ^{(1)} _{b},  & x ^{(3)} _{c} &= x ^{(3)} _{a} \cdot 3,         & x ^{(5)} _{c} &= x ^{(5)} _{a} + x ^{(5)} _{b},\\\\
        x ^{(2)} _{c} &= x ^{(2)} _{a} \cdot  x ^{(2)} _{b}, & x ^{(4)} _{c} &= x ^{(4)} _{a} + x ^{(4)} _{b}, & x ^{(6)} _{c} &= x ^{(6)} _{a} + 5
    \end{align}
$$

and wire constraints

$$
    \begin{align}
        u &= x_{a}^{(1)} = x_{a}^{(2)} = x_{b}^{(1)}, &t^{(1)} &= x_{a}^{(4)} = x_{c}^{(1)}, &t^{(3)} &= x_{b}^{(4)} = x_{c}^{(3)}, &t^{(5)} &= x_{a}^{(6)} = x_{c}^{(5)},\\\\
        v &= x_{b}^{(2)} = x_{b}^{(5)}, &t^{(2)} &= x_{a}^{(3)} = x_{c}^{(2)}, &t^{(4)} &= x_{a}^{(5)} = x_{c}^{(4)}, &t^{(6)} &= x_{c}^{(6)}.
    \end{align}
$$

We note that \\(x_b^{(3)}, x_b^{(6)}\\) are not set. To deal with these values, we simple set them to be equal to any random vales, since they do not affect the constraints defined above.

Notice that each equation in gate constrains receives either \\(2\\) secret inputs or \\(1\\) secret input and \\(1\\) constant and returns \\(1\\) secret output. Therefore, we use \\(2\\) columns, namely, \\(\mathsf{advice}_0\\) and \\(\mathsf{advice}_1\\), to contain the secret inputs and \\(1\\) column, namely, \\(\mathsf{advice}_2\\), to contain the secret output. Moreover, we also use the column \\(\mathsf{constant}\\) to contain possible public constants.

The remaining columns, namely, \\(\mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}}\\), are employed to contain the selectors indicating the required constraints for each row, which corresponds to a constraint in the set of gate constraints specified above. We now clarify the employment of selectors to guarantee the correctness of gate constraints.

## Specifying Table Values and Gate Constraints
For each row \\(i \in \\{1, \dots,  6\\}\\) of the table, we denote by the tuple 
$$
    (x_{a}^{(i)}, x_{b}^{(i)}, x_{c}^{(i)}, c^{(i)}, s_{\mathsf{add}}^{(i)}, s_{\mathsf{mul}}^{(i)}, s_{\mathsf{addc}}^{(i)}, s_{\mathsf{mulc}}^{(i)})
$$
corresponding to the tuple of columns 
$$
    (\mathsf{advice} _0, \mathsf{advice} _1, \mathsf{advice} _2, \mathsf{constant}, \mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}})
$$
devised above.

For each row \\(i \in \\{1,\dots, 6\\}\\), we set the following \\(4\\) constraints
$$
    \begin{align}
        s_\mathsf{add}^{(i)}\cdot (x_{a}^{(i)} + x_{b}^{(i)} - x_{c}^{(i)}) &= 0, & s_\mathsf{mul}^{(i)}\cdot (x_{a}^{(i)} \cdot x_{b}^{(i)} - x_{c}^{(i)}) &= 0,\\\\
        s_\mathsf{addc}^{(i)}\cdot (x_{a}^{(i)} + c^{(i)} - x_{c}^{(i)}) &= 0, & s_\mathsf{mulc}^{(i)}\cdot (x_{a}^{(i)} \cdot c^{(i)} - x_{c}^{(i)}) &= 0.
    \end{align}
$$

> **Example.** Assume that the \\(i\\)-th row corresponds to a multiplication gate. Hence, in this case, we set \\((s_{\mathsf{add}}^{(i)}, s_{\mathsf{mul}}^{(i)}, s_{\mathsf{addc}}^{(i)}, s_{\mathsf{mulc}}^{(i)}) = (0, 1, 0, 0)\\). We can see that only \\(s_{\mathsf{mul}}^{(i)} = 1\\) while remaining selectors are set to \\(0\\). 
>
> Therefore, whatever the values \\(x_{a}^{(i)}, x_{b}^{(i)}, x_{c}^{(i)}, c^{(i)}\\) are set, the results always hold with respect to the gates \\(s_\mathsf{add}^{(i)}\cdot (x_{a}^{(i)} + x_{b}^{(i)} - x_{c}^{(i)}) = 0, s_\mathsf{addc}^{(i)}\cdot (x_{a}^{(i)} + c^{(i)} - x_{c}^{(i)}) = 0\\) and \\(s_\mathsf{mulc}^{(i)}\cdot (x_{a}^{(i)} \cdot c^{(i)} - x_{c}^{(i)}) = 0\\). 
>
> However, since \\(s_{\mathsf{mul}}^{(i)} = 1\\), we can see that the gate \\(s_\mathsf{mul}^{(i)}\cdot (x_{a}^{(i)} \cdot x_{b}^{(i)} - x_{c}^{(i)}) = 0\\) must guarantee \\(x_{a}^{(i)} \cdot x_{b}^{(i)} = x_{c}^{(i)}\\).