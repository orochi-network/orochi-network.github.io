### Gate constraints
At this stage, for each \\(i \in \\{1,\dots, n\\}\\), we need to transform the computation of each gate to a unified form as follows:
$$q^O_i \cdot x_{c_i} + q^L_i \cdot x_{a_i} + q^R_i \cdot x_{b_i} + q^M_i \cdot (x_{a_i} \cdot x_{b_i}) + q^C_i = 0$$
where \\(q_i^O, q_i^L, q_i^R, q_i^M, q_i^C\\) are selectors uniquely determined by the corresponding gate. In particular,
- For addition gate, \\((q_i^O, q_i^L, q_i^R, q_i^M, q_i^C) = (-1, 1, 1, 0, 0)\\) since \\((-1) \cdot x_{c_i} + 1 \cdot x_{a_i} + 1 \cdot x_{b_i} + 0 \cdot (x_{a_i} \cdot x_{b_i}) + 0 = 0\\) is equivalent to \\(x_{c_i} = x_{a_i} + x_{b_i}\\).

- For multiplication gate, \\((q_i^O, q_i^L, q_i^R, q_i^M, q_i^C) = (-1, 0, 0, 1, 0)\\) since \\((-1) \cdot x_{c_i} + 0 \cdot x_{a_i} + 0 \cdot x_{b_i} + 1 \cdot (x_{a_i} \cdot x_{b_i}) + 0 = 0\\) is equivalent to \\(x_{c_i} = x_{a_i} \cdot x_{b_i}\\).

- For gate of addition with constant, \\((q_i^O, q_i^L, q_i^R, q_i^M, q_i^C) = (-1, 1, 0, 0, c)\\) since \\((-1) \cdot x_{c_i} + 1 \cdot x_{a_i} + 0 \cdot x_{b_i} + 0 \cdot (x_{a_i} \cdot x_{b_i}) + c = 0\\) is equivalent to \\(x_{c_i} = x_{a_i} + c\\).

- For gate of multiplication with constant, \\((q_i^O, q_i^L, q_i^R, q_i^M, q_i^C) = (-1, c, 0, 0, 0)\\) since \\((-1) \cdot x_{c_i} + c \cdot x_{a_i} + 0 \cdot x_{b_i} + 0 \cdot (x_{a_i} \cdot x_{b_i}) + 0 = 0\\) is equivalent to \\(x_{c_i} = x_{a_i} \cdot c\\).

> We now take a look at the example achieved above, i.e.,
> $$
    \begin{cases}
        x_{c_1} = x_{a_1} \cdot x_{b_1},\\\\
        x_{c_2} = x_{a_2} \cdot x_{b_2},\\\\
        x_{c_3} = x_{a_3} \cdot 3,\\\\
        x_{c_4} = x_{a_4} + x_{b_4},\\\\
        x_{c_5} = x_{a_5} + x_{b_5}, \\\\
        x_{c_6} = x_{a_6} + 5.
    \end{cases}
> $$ In this example, we can transform the above system of equation into the unified form as follows:
> $$ 
    \begin{cases}
        (-1) \cdot x_{c_1} + 0 \cdot x_{a_1} + 0 \cdot x_{b_1} + 1 \cdot (x_{a_1} \cdot x_{b_1}) + 0 = 0,\\\\
        (-1) \cdot x_{c_2} + 0 \cdot x_{a_2} + 0 \cdot x_{b_2} + 1 \cdot (x_{a_2} \cdot x_{b_2}) + 0 = 0,\\\\
        (-1) \cdot x_{c_3} + 3 \cdot x_{a_3} + 0 \cdot x_{b_3} + 0 \cdot (x_{a_3} \cdot x_{b_3}) + 0 = 0,\\\\
        (-1) \cdot x_{c_4} + 1 \cdot x_{a_4} + 1 \cdot x_{b_4} + 0 \cdot (x_{a_4} \cdot x_{b_4}) + 0 = 0,\\\\
        (-1) \cdot x_{c_5} + 1 \cdot x_{a_5} + 1 \cdot x_{b_5} + 0 \cdot (x_{a_5} \cdot x_{b_5}) + 0 = 0,\\\\
        (-1) \cdot x_{c_6} + 1 \cdot x_{a_6} + 0 \cdot x_{b_6} + 0 \cdot (x_{a_6} \cdot x_{b_6}) + 5 = 0.
    \end{cases}
> $$