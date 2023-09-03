## Definition and Security

In this section, we describe the syntax and security properties of a threshold signature scheme.

### Definition

We describe the syntax of a threshold signature scheme. A \\((t-n)\\) threshold signature consists of two protocols **KeyGen**, **Sign** and an algorithm **Verify** as follow: 

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):** This is an interactive protocol between \\(n\\) participants \\(P_1,P_2,\dots,P_n\\). If the interaction suceeds, each participant \\(P_i\\) receives a secret key \\(sk_i\\). In addition, all participants output a common public key \\(pk\\). 

**Sign\\((M,\mathcal{S})\langle \\{P_i(sk_i)\\}_{i \in \mathcal{S}}\rangle\\):** This is an interactive protocol with common input \\(M\\) between a set \\(\mathcal{S}\\) of \\(t+1\\)  participants \\(\\{P_i\\}_{i \in \mathcal{S}}\\), where each participant \\(P_i\\) holds a secret key \\(sk_i\\) only known to him. If the interaction suceeds, the protocol outputs a signature \\(\sigma\\).

**Verify\\((M,\sigma,pk)\\):** This is an algorithm run by an external verifier to check the correctness of the signature. On input a message \\(M\\), a pair of signature \\((\rho,\sigma)\\), a common public key \\(pk\\), it outputs a bit b indicating the validity of the signature.

### Security Properties

A threshold signature scheme should satisfy the following properties:

**Correctness:** For any set \\(\mathcal{S} \subset \\{1,\dots,n\\}\\) with \\(|\mathcal{S}|=t+1\\), if \\(P_i\\) follows the protocol for all \\(i \in \mathcal{S}\\) and 
\\(\sigma \leftarrow\\) **Sign\\((M,\mathcal{S})\langle \\{P_i(sk_i)\\}_{i \in \mathcal{S}}\rangle\\)**, then it holds that **Verify\\(((M,\sigma,pk)=1\\)**.

**Unforgability:** A \\((t-n)\\) threshold signature scheme is unforgeable if for any adversary \\(\mathcal{A}\\) who corrupts up to \\(t\\) participants and given previous signatures \\(\sigma_1,\dots,\sigma_k\\) of previous messages \\(M_1,\dots,M_k\\), cannot produce a valid signature \\(\sigma\\) on an unsigned message \\(M\\) except with negligible probability.