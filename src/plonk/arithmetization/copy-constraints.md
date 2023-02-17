# Copy Constraints
Recall that gate constraints do not enforce the equalities of wire values making inconsistencies across the circuit. We generalize copy constraints to the following problem.

Let \\(k \in \\{1, \dots, 3n\\}\\) and \\(\\{i_1, \dots, i_k\\} \subseteq \mathcal{I}\\) satisfying \\(i_1 < i_2 < \dots < i_k\\). We would like to show that
$$
    x_{i_1} = x_{i_2} = \dots = x_{i_k}.
$$

The technique for proving this problem is tricky. We form the pairs of index-value and make them into a sequence 
$$
    \big((i_1, x_{i_1}), (i_2, x_{i_2}), \dots, (i_k, x_{i_k})\big).
$$
It can be observed that if \\(x_{i_1} = x_{i_2} = \dots = x_{i_k}\\), then, if we rotate the indices among the pairs, we achieve a sequence  
$$
    \big((i_k, x_{i_1}), (i_1, x_{i_2}), \dots, (i_{k - 1}, x_{i_k})\big)
$$
that is permutation of the previous sequence. Notice that we just rotated the indices \\(1\\) step to the left and this is the recommended rotation. This fact helps imply the other direction of the fact. For more details, we use the following observation

**Observation.** \\(\big((i_1, x_{i_1}), (i_2, x_{i_2}), \dots, (i_k, x_{i_k})\big)\\) is a permutation of \\(\big((i_k, x_{i_1}), (i_1, x_{i_2}), \dots, (i_{k - 1}, x_{i_k})\big)\\) if and only if \\(x_{i_1} = x_{i_2} = \dots = x_{i_k}\\).

*Proof.* The proof is as follows:

"\\(\Leftarrow\\)": This direction is straightforward.

"\\(\Rightarrow\\)": Since the two sequences are permutation of each other, for \\(j \in \\{1, \dots, k\\}\\) we consider \\((i_j, x_{i_j})\\) from the first sequence. It can be seen that \\((i_j, x_{i_{(j \mod k) + 1}})\\) from the second sequence is the only one that is equal \\(j \in \\{1, \dots, k\\}\\) since they share the unique prefix \\(i_j\\). Hence, \\(x_{i_j} = x_{i_{(j \mod k) + 1}}\\). Following this argument, we see that \\(x_{i_1} = x_{i_2}, x_{i_2} = x_{i_3}, \dots, x_{i_{k - 1}} = x_{i_k}, x_{i_k} = x_{i_1}\\). Thus, \\(x_{i_1} = x_{i_2} = \dots = x_{i_k}\\).

#### General paradigm for proving copy constraints of the entire circuit
Recall that \\(x : \mathcal{I} \to \mathbb{F}\\). We can deterministically determine a partition of \\(\mathcal{I}\\) such that 
$$
    \mathcal{I} = \bigcup_{j = 1}^{\ell_{\mathsf{in}} + n} \mathcal{I}_j
$$ 

where \\(\ell_{\mathsf{in}} + n\\) is the number of wires of the original circuits, namely, \\(\ell_{\mathsf{in}}\\) input wires and \\(n\\) output wires of all gates. Hence each subset \\(\mathcal{I}_j\\) is the set of wire labels whose value are all equal to the same wire value of the original circuit. We hence obtain a rotation of indices for each subset \\(\mathcal{I}_j\\). By unifying all those rotations, we achieve a permutation \\(\sigma : \mathcal{I}\to\mathcal{I}\\) such that

$$
    \big((a_1, x_{a_1}), \dots, (a_n, x_{a_n}), (b_1, x_{b_1}), \dots, (b_n, x_{b_n}), (c_1, x_{c_1}), \dots, (c_n, x_{c_n})\big)
$$

is a permutation of

$$
    \big((\sigma(a_1), x_{a_1}), \dots, (\sigma(a_n), x_{a_n}), (\sigma(b_1), x_{b_1}), \dots, (\sigma(b_n), x_{b_n}), (\sigma(c_1), x_{c_1}), \dots, (\sigma(c_n), x_{c_n})\big).
$$
Such guaranteed permutation relation implies the consistencies among wires of the circuit.

> Recall the example in [Breaking Circuit](./subsection_breaking_circuit.md) with the following copy constraints.
> $$
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
> We achieve the partition
> $$
    \big\\{\\{a_1, a_2, b_1\\}, \\{b_2, b_5\\}, \\{a_4, c_1\\}, \\{a_3, c_2\\}, \\{b_4, c_3\\}, \\{a_5, c_4\\}, \\{a_6, c_5\\}, \\{c_6\\}\big\\}.
> $$
> We hence can achive the permutation \\(\sigma: \mathcal{I}\to\mathcal{I}\\) as follows:
> $$
    \begin{array}[ccc]\\\\
        \sigma(a_1) = b_1, &\sigma(a_2) = a_1, &\sigma(b_1) = a_2,\\\\
        \sigma(b_2) = b_5, &\sigma(b_5) = b_2,\\\\
        \sigma(a_4) = c_1, &\sigma(c_1) = a_4,\\\\
        \sigma(a_3) = c_2, &\sigma(c_2) = a_3,\\\\
        \sigma(b_4) = c_3, &\sigma(c_3) = b_4,\\\\
        \sigma(a_5) = c_4, &\sigma(c_4) = a_5,\\\\
        \sigma(a_6) = c_5, &\sigma(c_5) = a_6,\\\\
        \sigma(c_6) = c_6.
    \end{array}
> $$