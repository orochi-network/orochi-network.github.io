# A Simple Arithmetic Circuit
Let \\(\mathbb{F}\\) be some finite field. We would like to show in this section the transformation from the program computing polynomial \\(f(u, v) = u^2 + 3uv + v + 5 \\) where \\(u, v \in \mathbb{F}\\). The arithmetic circuit for this polynomial is equivalently represented by topologically following the sequence of computations below.
1. Take input \\(u \in \mathbb{F}\\).
2. Take input \\(v \in \mathbb{F}\\).
3. Compute \\(u^2\\).
4. Compute \\(uv\\).
5. Compute \\(3uv\\).
6. Compute \\(u^2 + 3uv\\).
7. Compute \\(u^2 + 3uv + v\\).
8. Compute \\(u^2 + 3uv + v + 5\\).

