# Hades-based POSEIDON\\(^\pi\\) permutation design

## Notation
The function takes input of \\(t \geq 2\\) words in \\(\mathbb{F}_p\\), where $p$ is a prime of size \\(p \approx 2^n > 2^{30}\\) and \\(N = nt\\) to denote the size of texts in bits.

## The SBox layer
They focus on the setting: $$SB(x) = x^\alpha, \alpha \geq 3$$ where \\(\alpha\\) is the smallest positive integer that satisfies \\(gcd(\alpha, p - 1) = 1\\). These permutation are called "\\(x^\alpha-Poseidon^\pi\\)".

They use:
+ \\(\alpha = 3\\) if \\(p \not\equiv 1 \pmod{3}\\)
+ \\(\alpha = 5\\) if \\(p \equiv 1 \pmod{3}\\) and \\(p \not\equiv 1 \pmod{5}\\)

It turns out that \\(SB(x) = x^5\\) is suitable for the prime subfields of the scalar field of ***BLS12-381*** and ***BN254***, which are two of the most popular prime fields in ZK applications.

## MixColumns - The linear layer
It is proved {{#cite MS78}} that a \\(t \times t\\) MDS matrix with elements in \\(\mathbb{F}_p\\) exists if \\(2t + 1 \leq p\\).

One method of constructing such a matrix introduced in this paper is using a ***Cauchy matrix***. For \\((x_i, y_i) \in \mathbb{F}\_p\\), where \\(i \in [1, t]\\), the entries of the matrix \\(\mathcal{M}\\) are: $$\mathcal{M}\_{i, j} = \dfrac{1}{x_i + y_j}$$ where the entries of \\(\\{x_i\\}\_{1 \leq i \leq t}\\) and \\(\\{y_i\\}\_{1 \leq i \leq t}\\) are pairwise distinct and \\(x_i + y_i \neq 0\\), where \\(i \in \\{1,\cdots,t\\}\\) and \\(j \in \\{1,\cdots,t\\}\\)

## Construct the matrix
The paper suggest the following method to generate matrices to avoid some known vulnerabilities which this note skipped:
+ Randomly select pairwise distinct \\(\\{x_i\\}\_{1 \leq i \leq t}\\) and \\(\\{y_i\\}\_{1 \leq i \leq t}\\).
+ Determine if the matrix is secure using [this test](https://extgit.iaik.tugraz.at/krypto/linear-layer-tool) introduced in {{#cite GRS20}}
+ Repeat this procedure until we find a secure matrix.