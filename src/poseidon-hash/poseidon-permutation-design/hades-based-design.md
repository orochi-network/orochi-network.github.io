# Hades-based POSEIDON\\(^\pi\\) permutation design

Recall in the [previous section](./hades-design-strategy.md), the \\(\mathsf{Poseidon}^\pi\\) permutation is a composition of round functions \\(AddroundConstants\\), \\(SubWords\\) and \\(MixLayer\\), each takes input a state \\(S=(S_1,S_2,\dots,S_t)\\) and produces a new state \\(S'\\). In this Section, we describe the \\(3\\) main components of the round function of \\(\mathsf{Poseidon}^\pi\\).

## Notation
The function takes input of \\(t \geq 2\\) words in \\(\mathbb{F}_p\\), where \\(p\\) is a prime of size \\(p \approx 2^n > 2^{30}\\) and \\(N = nt\\) to denote the size of texts in bits.

## AddRoundConstants component

This is the first component of the round function described in [Hades design strategy](./hades-design-strategy.md). Given the input state \\( S = ( S_1, S_2, \cdots, S_t ) \\), where each of \\( S_i \\) is a word, it outputs $$ S' = AddRoundConstants(S, T) = S + T = ( S_1 + T_1, S_2 + T_2, \cdots, S_t + T_t ) $$ where \\(T\\) is the ***round constant vector*** that is randomly generated in a method described in [Poseidon concrete instantiation](./concrete-poseidon-instantiation.md). Essentially, this component performs a field addition between the state and the round constant.

## The SubWords (SBox) layer

This is the second component of the round function described in [Hades design strategy](./hades-design-strategy.md). ***Subtitution boxes*** (or ***S-Boxes***) are widely used in cryptography. In this context of an architecture that resembles block ciphers, SBoxes are used to ensure [Shannon's property of confusion](https://en.wikipedia.org/wiki/Confusion_and_diffusion).

Given the state \\( S = ( S_1, S_2, \cdots, S_t ) \\), where each of \\(S_i\\) is a word, this component outputs a new state $$ S' = SB(S) =  ( SB(S_1), SB(S_2), \cdots, SB(S_t) ).$$

In {{#cite GKRRS21}}, they focus on the setting: $$SB(x) = x^\alpha, \alpha \geq 3$$ where \\(\alpha\\) is the smallest positive integer that satisfies \\(gcd(\alpha, p - 1) = 1\\). These permutations are called "\\(x^\alpha-Poseidon^\pi\\)".

They use:
+ \\(\alpha = 3\\) if \\(p \not\equiv 1 \pmod{3}\\)
+ \\(\alpha = 5\\) if \\(p \equiv 1 \pmod{3}\\) and \\(p \not\equiv 1 \pmod{5}\\)

It turns out that \\(SB(x) = x^5\\) is suitable for the prime subfields of the scalar field of ***BLS12-381*** and ***BN254***, which are two of the most popular prime fields in ZK applications.

## MixColumns - The linear layer

This is the last layer (also called the ***linear layer***) of the round function. Given the state \\( S = ( S_1, S_2, \cdots, S_t ) \\). This layer outputs 
$$ S' = \mathcal{M} \times S =
\begin{bmatrix} 
    \mathcal{M}\_{11} & \mathcal{M}\_{12} & \cdots & \mathcal{M}\_{1t} \\\ 
    \mathcal{M}\_{21} & \mathcal{M}\_{22} & \cdots & \mathcal{M}\_{2t} \\\ 
    \cdots & \cdots & \cdots & \cdots \\\ 
    \mathcal{M}\_{t1} & \mathcal{M}\_{t2} & \cdots & \mathcal{M}\_{tt} 
\end{bmatrix} \times \begin{bmatrix}
    S_1 \\\ S_2 \\\ \cdots \\\ S_t
\end{bmatrix} $$ 
where \\(\mathcal{M}\\) is the secure ***Maximum Distance Separable*** matrix of size \\(t \times t\\), which is explained in [the Hades design strategy](./hades-design-strategy.md). The MDS matrices are randomly and securely generated using a method described in [Poseidon concrete instantiations](./concrete-poseidon-instantiation.md).

It is proved {{#cite MS78}} that a \\(t \times t\\) MDS matrix with elements in \\(\mathbb{F}_p\\) exists if \\(2t + 1 \leq p\\).

One method of constructing such a matrix introduced in this paper is using a ***Cauchy matrix***. For \\((x_i, y_i) \in \mathbb{F}\_p\\), where \\(i \in [1, t]\\), the entries of the matrix \\(\mathcal{M}\\) are: $$\mathcal{M}\_{i, j} = \dfrac{1}{x_i + y_j}$$ where the entries of \\(\\{x_i\\}\_{1 \leq i \leq t}\\) and \\(\\{y_i\\}\_{1 \leq i \leq t}\\) are pairwise distinct and \\(x_i + y_i \neq 0\\), where \\(i \in \\{1,\cdots,t\\}\\) and \\(j \in \\{1,\cdots,t\\}\\)

## Constructing the matrix
The paper suggest the following method to generate matrices to avoid some known vulnerabilities which this note skipped:
+ Randomly select pairwise distinct \\(\\{x_i\\}\_{1 \leq i \leq t}\\) and \\(\\{y_i\\}\_{1 \leq i \leq t}\\) where \\(x_i + y_j \neq 0\\) and where \\(i \in \\{ 1,\cdots,t \\}\\) and \\(j \in \\{ 1,\cdots,t \\}\\).
+ Determine if the matrix is secure using [this test](https://extgit.iaik.tugraz.at/krypto/linear-layer-tool) introduced in {{#cite GRS20}}
+ Repeat this procedure until we find a secure matrix.