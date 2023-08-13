### Definition

A threshold signature consists of two protocols **KeyGen**, **Sign** and an algorithm **Verify** as follow: 

**Keygen:** This is an interactive protocol between \\(n\\) participants \\(P_1,P_2,\dots,P_N\\). If the interaction suceeds, each participant \\(P_i\\) receives a secret key \\(sk_i\\). In addition, all participants output a common public key \\(pk\\). 

**Verify:** This is an algorithm run by an external verifier to check the correctness of the signature. On input a message \\(M\\), a pair of signature \\((\rho,\sigma)\\), a common public key \\(pk\\), it outputs a bit b indicating the validity of the signature.