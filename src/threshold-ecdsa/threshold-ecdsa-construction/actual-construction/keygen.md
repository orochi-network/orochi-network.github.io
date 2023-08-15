### Key Generation

We describe the key generation process in the construction of Gennaro et al. This protocol is divided into two sub protocols: **KeyGen** and **KeyRefresh**. In the **KeyGen** protocol, participants jointly follow the protocol to generate the common public key \\(pk\\) for vefification and a list of secret keys \\(\\{sk_i\\}_{i=1}^n\\) for each participant. The **KeyRefresh** protocol is used to renew the keys of the participants.

**KeyGen:**
- Each \\(P_i\\) sample \\(sk_i\\) and sets \\(pk_i=g^{sk_i}\\)

- Each \\(P_i\\) broadcast a Schnoor proof that he know the exponent \\(sk_i\\) given the public key \\(pk_i\\). 

- The public key \\(pk\\) is set to \\(pk=\prod_{i=1}^npk_i\\).

**KeyRefresh:**

Each participant \\(P_i\\) does the following:

- Sample two safe primes \\(p_i,q_i\\) and let \\(N_i=p_i\cdot q_i\\).

- Sample \\(\mathbf{x_i}=(x_{i,1},x_{i,2},\dots,x_{i,n})\\) satisfying \\(\sum_{j=1}^{n}x_{i,j}=0\\) and set \\(\mathbf{X_i}=(X_{i,1}=g^{x_{i,1}},\dots,X_{i,n}=g^{x_{i,n}})\\)

- Finally, the new secret key \\(sk^*_i\\) of \\(P_i\\) is set to be:

$$sk_i^*=sk_i+\sum_{j=1}^{n}x_{j,i}$$