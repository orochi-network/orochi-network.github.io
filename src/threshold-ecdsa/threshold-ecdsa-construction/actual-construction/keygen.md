### Key Generation

In this section, we describe the key generation process in the construction of Gennaro et al. The key generation process is divided into two sub protocols: the initial key generation process and the key refresh process.
The initial key generation process is executed exactly once to produce a public-secret key pair \\(pk,sk\\), while the key refresh process is executed whenever participants would like to change their partial secret keys in the way that \\(pk\\) and \\(sk\\) remains the same.
 Before moving to the protocol, we provide several notations that will be used in the protocol description below.  

**Notation:** Let \\(\lambda\\) to be the security parameter. Let \\(\mathbb{G}\\) to be a cyclic group whose order is a prime number. Let \\(p \in (2^{\lambda-1},2^\lambda)\\) to be the order of \\(\mathbb{G}\\) and let \\(g,h\\) to be two generators of \\(\mathbb{G}\\). We denote \\(\mathsf{Com}\\) to be a secure binding and information theoretic hiding commitment scheme and \\(\mathsf{H}\\) to be a cryptographic hash function. For any set \\(\mathcal{S}\\) and for any \\(i \in \mathcal{S}\\) we denote \\(\lambda_{i,S}=\prod_{j\in \mathcal{S},j \neq i}\dfrac{j}{j-i}\\) to be the Lagrange coefficient w.r.t \\(S\\).

Now, the initial key generation and key-refresh process are as follows:

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):**

**Initial Key Generation:**
The initial key generation is executed once at the beginning.

1. Each participant \\(P_i\\) selects \\(s_i \in Z_p \\) and compute \\(C_i=\mathsf{Com}(g^{s_i})\\).

2. Each participant \\(P_i\\) broadcasts \\(y_i=g^{s_i}\\). The public key \\(pk\\) is set to be \\(pk=\prod_{i=1}^ny_i\\). \\(P_i\\) then performs Feldman's Verifiable Secret Sharing scheme (see [Supporting Protocols](./supporting-algorithms.md)) to share \\(s_i\\) to other participants.  Each \\(P_j\\) add the secret shares received as his secret key, i.e, \\(sk_j=\sum_i s_{ij}\\). The values \\(sk_i\\) are the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\).

3. Finally, each participant uses Schnorr's protocol {{#cite S91}} (see [Supporting Protocols](./supporting-algorithms.md)) to prove in zero knowledge that he knows the secret key \\(sk_i\\), 

**Key Refresh:**
The key refreshment process is executed after a certain number of epochs whenever participants have to reset their partial secret keys.
 
1. Each participant \\(P_i\\) samples \\(E_i=(N_i,h_{i1},h_{i2})\\), the public key of Pallier Cryptosystem {{#cite P91}} satisfying \\(N_i>2^{8\lambda}\\).

2. Each participant \\(P_i\\) performs Feldman's Verifiable Secret Sharing scheme to distribute the shares \\(s_{ij}'\\) of \\(0\\) to other participants. Each participant \\(P_i\\) set his new secret key to be \\(sk_i'=sk_i+\sum_i s_{ji}'\\). The secret key \\(sk\\) remains the same and are unknown to other participants and the values \\(sk_i'\\) are still the shares of a \\((t-n)\\) Shamir secret sharing of the secret key \\(sk\\)

3. Finally, each participant does the following:
    1. Use Schnorr's protocol to prove in zero knowledge that he knows the new secret key \\(sk_i'\\).

    2. Prove that \\(N_i\\) is a product of two primes \\(p_i,q_i\\) s.t \\(p_i \equiv q_i \equiv 3 \pmod {4}\\) and \\(N_i\\) admits no small factors (see [Supporting Protocols](./supporting-algorithms.md))

    3. Prove that \\((h_{i1},h_{i2})\\) generates the same multiplicative group modulo \\(N_i\\) using Schnorr protocol for Ring (see [Supporting Protocols](./supporting-algorithms.md)).
 

By the property of Feldman's VSS, it can be proven that the public key \\(pk\\) is also equal to \\(g^{sk}\\), hence the key pair \\((pk,sk)\\) generated using the key generation protocol above has the same form of a key pair in an ECDSA signature scheme. 

**Note:** Note that after the key generation process, we see that each \\(P_i\\) is now equipped with a Pallier encryption scheme with public key \\(E_i=(N_i,h_{i1},h_{i2})\\), which we denote the encryption and decryption algorithm by \\(\mathsf{Enc}_i\\) and \\(\mathsf{Dec}_i\\) respectively. The encryption algorithm receives two inputs: the message \\(M\\) and a randomness \\(\rho\\). In most cases, for simplicity we will ignore the input randomness \\(\rho\\) in our description. The encryption scheme will be used in the signing process.