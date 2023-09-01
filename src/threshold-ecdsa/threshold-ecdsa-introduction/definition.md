### Definition

A threshold signature consists of two protocols **KeyGen**, **Sign** and an algorithm **Verify** as follow: 

**Keygen \\((1^\lambda)\langle \\{P_i\\}_{i=1}^n\rangle\\):** This is an interactive protocol between \\(n\\) participants \\(P_1,P_2,\dots,P_N\\). If the interaction suceeds, each participant \\(P_i\\) receives a secret key \\(sk_i\\). In addition, all participants output a common public key \\(pk\\). 

**Sign\\((M)\langle \\{P_i(sk_i)\\}_{i=1}^n\rangle\\):** This is an interactive protocol on a common input \\(M\\) between \\(n\\) participants \\(P_1,P_2,\dots,P_N\\), where each participant \\(P_i\\) holds a secret key \\(sk_i\\)  only known to him. At the end of the protocol, the protocol outputs a signature \\(\sigma\\).

**Verify\\((M,\sigma,pk)\\):** This is an algorithm run by an external verifier to check the correctness of the signature. On input a message \\(M\\), a pair of signature \\((\rho,\sigma)\\), a common public key \\(pk\\), it outputs a bit b indicating the validity of the signature.

