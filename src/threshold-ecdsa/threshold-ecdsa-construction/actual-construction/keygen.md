### Key Generation

In this section, we describe the key generation process in the construction of Gennaro et al. The key generation process is follow.

- Each participant \\(P_i\\) select \\(s_i \in \mathbb{Z}_p\\) and compute \\(C_i=\mathsf{Com}(g^{s_i})\\) and broadcasts \\(E_i\\), the public key for Pallier's cryptosystem.

- Each participant \\(P_i\\) broadcasts \\(y_i=g^{s_i}\\). The public key \\(pk\\) is set to be \\(pk=\prod_{i=1}^ny_i\\). \\(P_i\\) then performs Feldman's VSS to share \\(s_i\\).  Each \\(P_j\\) add the secret shares received as his secret key, i.e, \\(sk_j=\sum_i s_{ij}\\). The values \\(sk_i\\) are the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\).

- Each participant \\(P_i\\) use Schnorr's protocol to prove that he knows the secret key \\(sk_i\\).