### Key Generation

In this section, we describe the key generation process in the construction of FROST below. 

**Notation:** Let \\(\lambda\\) to be the security parameter. Let \\(\mathbb{G}\\) to be a cyclic group whose order is a prime number. Let \\(p \in (2^{\lambda-1},2^\lambda)\\) to be the order of \\(\mathbb{G}\\) and let \\(g,h\\) to be two generators of \\(\mathbb{G}\\). We denote \\(\mathsf{Com}\\) to be a secure binding and information theoretic hiding commitment scheme and \\(\mathsf{H}\\) to be a cryptographic hash function. For any set \\(\mathcal{S}\\) and for any \\(i \in \mathcal{S}\\) we denote \\(\lambda_{i,S}=\prod_{j\in \mathcal{S},j \neq i}\dfrac{j}{j-i}\\) to be the Lagrange coefficient w.r.t \\(S\\).


**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):**

**Initial Key Generation:**
The initial key generation is executed once at the beginning.

1. Each participant \\(P_i\\) select \\(s_i \in Z_p \\) and compute \\(C_i=\mathsf{Com}(g^{s_i})\\).

2. Each participant \\(P_i\\) broadcasts \\(y_i=g^{s_i}\\). The public key \\(pk\\) is set to be \\(pk=\prod_{i=1}^ny_i\\). \\(P_i\\) then performs Feldman's Verifiable Secret Sharing scheme (see [Supporting Protocols](./supporting-algorithms.md)) to share \\(s_i\\) to other participants.  Each \\(P_j\\) add the secret shares received as his secret key, i.e, \\(sk_j=\sum_i s_{ij}\\). The values \\(sk_i\\) are the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\).

3. Finally, each participant use Schnorr's protocol {{#cite S91}} (see [Supporting Protocols](./supporting-algorithms.md)) to prove in zero knowledge that he knows the secret key \\(sk_i\\), 



By the property of Feldman's VSS, it can be proven that the public key \\(pk\\) is also equal to \\(g^{sk}\\), hence the key pair \\((pk,sk)\\) generated using the key generation protocol above has the same form of a key pair in a Schnorr signature scheme. 

