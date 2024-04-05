# Hades design strategy

## Overview
In \\(\mathsf{Hades}\\), they mix rounds with ***full SBox layers*** and rounds with ***partial SBox layers***. They want to strike the balance between:
+ Full layers are ***expensive*** in software and ZK proof systems, but ***well resists*** statistical attacks.
+ Partial layers are ***computationally cheap***, but often are vulnerable against algebraic attacks.

## Details

The Hades design strategy consists of $R_f$ initial rounds, in which SBoxes are applied to the full state. After these $R_f$ rounds, $R_p$ rounds in the middle contain a single SBox for each round, and the rest of the state goes through the nonliner layer ***unchanged*** (you can say that the rest goes through the identity functions). Finally, $R_f$ rounds at the end are applied again:
$$R_f \longrightarrow R_p \longrightarrow R_f$$
![Hades](https://docs.polygon.technology/img/zkEVM/01psd-hades-based-poseidon-perm.png)

## The round function

Each round function consists of \\(3\\) components:
+ \\(AddRoundConstants\\), denoted by \\(ARC(\cdot)\\), 
+ \\(SubWords\\), denoted by \\(SBox(\cdot)\\) or \\(SB(\cdot)\\).
+ \\(MixLayers\\), denoted by \\(M(\cdot)\\). This is the ***linear layer*** of the construction. It involves multiplication between the state and a \\(t \times t\\) ***MDS(Maximum Distance Separable) matrix***.

## Maximum Distance Saparable

A matrix \\(M \in \mathbb{F}^{t \times t}\\) is called ***maximum distance saparable (MDS)*** iff it has a branch number \\(\mathcal{B}(M) = t + 1\\). The branch number of \\(M\\) is defined as \\(\mathcal{B}(M) = min_{x \in \mathbb{F}^t}\\{wt(x) + wt(M(x))\\}\\) where \\(wt\\) is the ***Hamming weight*** in wide trail terminology. 

Equivalently, a matrix \\(M\\) is MDS iff **every submatrix of \\(M\\) is non-singular** (a non-singluar matrix is a matrix whose determinant is not \\(0\\)).

There will be a construction of these matrices in later sections.