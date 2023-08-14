### Key Generation

We describe the key generation process in the construction of Gennaro et al. This protocol is divided into two sub protocols: **KeyGen** and **KeyRefresh**. In the **KeyGen** protocol, participants jointly follow the protocol to generate the common public key \\(pk\\) for vefification and a list of secret keys \\(\\{sk_i\\}_{i=1}^n\\) for each participant. The **KeyRefresh** protocol is used to renew the keys of the participants.

**KeyGen:**
- Each \\(P_i\\) sample \\(sk_i\\) and sets \\(pk_i=g^{sk_i}\\)
- 
- The public key \\(pk\\) is set to \\(pk=\prod_{i=1}^npk_i\\)

**KeyRefresh:**

Each participant \\(P_i\\) does the following:

- Sample two safe primes \\(p_i,q_i\\) and let \\(N_i=p_i\cdot q_i\\).