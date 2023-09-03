### Key Generation

In this section, we describe the key generation process in the construction of Gennaro et al. Before moving to the protocol, we provide several notations that will be used in the protocol description below.  

**Notation:** Let \\(\mathbb{G}\\) to be a cyclic group whose order is a prime number. Let \\(p\\) to be the order of \\(\mathbb{G}\\) and let \\(g,h\\) to be two generators of \\(\mathbb{G}\\). We denote \\(\mathsf{Com}\\) to be a secure binding and information theoretic hiding commitment scheme and \\(\mathsf{H}\\) to be a cryptographic hash function. For any set \\(\mathcal{S}\\) and for any \\(i \in \mathcal{S}\\) we denote \\(\lambda_{i,S}=\prod_{j\in \mathcal{S},j \neq i}\dfrac{j}{j-i}\\) to be the Lagrange coefficient w.r.t \\(S\\).

Now, the key generation process is follow.

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):**

1. Each participant \\(P_i\\) select \\(s_i \in Z_p \\) and compute \\(C_i=\mathsf{Com}(g^{s_i})\\) and broadcasts \\(E_i=(N_i,h_{i1},h_{i2})\\), the public key of Pallier's cryptosystem, where \\(N_i>p^8\\). The Pallier's cryptosystem will be used in the \\(\mathsf{MtA}\\) protocol in [Supporting Protocols](./supporting-algorithms.md).

2. Each participant \\(P_i\\) broadcasts \\(y_i=g^{s_i}\\). The public key \\(pk\\) is set to be \\(pk=\prod_{i=1}^ny_i\\). \\(P_i\\) then performs Feldman's Verifiable Secret Sharing scheme (see [Supporting Protocols](./supporting-algorithms.md)) to share \\(s_i\\) to other participants.  Each \\(P_j\\) add the secret shares received as his secret key, i.e, \\(sk_j=\sum_i s_{ij}\\). The values \\(sk_i\\) are the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\).

3. Let \\(N_i\\) be the  RSA modulus associated with \\(E_i\\). Each participant \\(P_i\\) do the following:
 - Use Schnorr's protocol {{#cite S91}} to prove in zero knowledge that he knows the secret key \\(sk_i\\), 
 - Prove that \\(N_i\\) is **a product of two safe primes** using the proof system of Gennaro, Micciancio, and Rabin {{#cite GMR98}} and that\\(h_{i1},h_{i2}\\) generate the same group modulo \\(N_i\\). 
 

By the property of Feldman's VSS, it can be proven that the public key \\(pk\\) is also equal to \\(g^{sk}\\), hence the key pair \\((pk,sk)\\) generated using the key generation protocol above has the same form of a key pair in an ECDSA signature scheme.