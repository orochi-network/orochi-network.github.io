### Definition

A \\((t-n)\\) threshold signature consists of two protocols **KeyGen**, **Sign** and an algorithm **Verify** as follow: 

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):** This is an interactive protocol between \\(n\\) participants \\(P_1,P_2,\dots,P_n\\). If the interaction suceeds, each participant \\(P_i\\) receives a secret key \\(sk_i\\). In addition, all participants output a common public key \\(pk\\). 

**Sign\\((M,\mathcal{S})\langle \\{P_i(sk_i)\\}_{i \in \mathcal{S}}\rangle\\):** This is an interactive protocol with common input \\(M\\) between a set \\(\mathcal{S}\\) of \\(t+1\\)  participants \\(\\{P_i\\}_{i \in \mathcal{S}}\\), where each participant \\(P_i\\) holds a secret key \\(sk_i\\) only known to him. If the interaction suceeds, the protocol outputs a signature \\(s\\).

**Verify\\((M,s,pk)\\):** This is an algorithm run by an external verifier to check the correctness of the signature. On input a message \\(M\\), a pair of signature \\((\rho,\sigma)\\), a common public key \\(pk\\), it outputs a bit b indicating the validity of the signature.

