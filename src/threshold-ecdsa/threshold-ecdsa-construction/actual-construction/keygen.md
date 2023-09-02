### Key Generation

In this section, we describe the key generation process in the construction of Gennaro et al. In the protocol, we denote \\(\mathsf{Com}\\) to be a secure bindning and information theoretic hiding commitment scheme. The key generation process is follow.

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):**

1. Each participant \\(P_i\\) select \\(s_i \in \mathbb{Z}_p\\) and compute \\(C_i=\mathsf{Com}(g^{s_i})\\) and broadcasts \\(E_i\\), the public key for Pallier's cryptosystem. The Pallier's cryptosystem will be used in the \\(\mathcal{MtA}\\) protocol in [Supporting Protocols](./supporting-algorithms.md).

2. Each participant \\(P_i\\) broadcasts \\(y_i=g^{s_i}\\). The public key \\(pk\\) is set to be \\(pk=\prod_{i=1}^ny_i\\). \\(P_i\\) then performs Feldman's VSS to share \\(s_i\\) to other participants.  Each \\(P_j\\) add the secret shares received as his secret key, i.e, \\(sk_j=\sum_i s_{ij}\\). The values \\(sk_i\\) are the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\).

3. Finally, each participant \\(P_i\\) use Schnorr's protocol to prove that he knows the secret key \\(sk_i\\).