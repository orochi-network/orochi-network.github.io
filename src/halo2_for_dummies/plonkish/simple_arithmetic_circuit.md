# A Simple Arithmetic Circuit
Let \\(\mathbb{F}\\) be some finite field. We would like to show in this section the transformation from the program computing polynomial \\(u^2 + 3uv + v + 5 \\) where \\(u, v \in \mathbb{F}\\). The arithmetic circuit for this polynomial is equivalently represented by topologically following the sequence of computations below.
1. Take input \\(u \in \mathbb{F}\\).
2. Take input \\(v \in \mathbb{F}\\).
3. Compute \\(u^2\\).
4. Compute \\(uv\\).
5. Compute \\(3uv\\).
6. Compute \\(u^2 + 3uv\\).
7. Compute \\(u^2 + 3uv + v\\).
8. Compute \\(u^2 + 3uv + v + 5\\).

To setup the above sequence of computations into PLONKish arithmetization, we specify a table to contains all possible variable appeared during the computing the arithmetic circuit fir \\(u^2 + 3uv + v + 5\\). Then, for each row, we setup a few gates, or equivalently gate constraints, that applies to the row. Specificially, we define the following tuple of columns
$$
    (\mathsf{advice} _0, \mathsf{advice} _1, \mathsf{advice} _2, \mathsf{constant}, \mathsf{selector} _{\mathsf{add}}, \mathsf{selector} _{\mathsf{mul}}, \mathsf{selector} _{\mathsf{addc}}, \mathsf{selector} _{\mathsf{mulc}}).
$$