# Threshold Signature

In this chapter, we give an overview of threshold signatures and describe the construction of Canetti et al in {{#cite CGGMP21}}. The chapter is separated into \\(3\\) major parts below:
- First, we give a brief introduction to threshold signatures and state its syntax and security properties in [Introduction](./threshold-ecdsa-introduction/introduction.md). 
- Second, we describe the threshold ecdsa construction of {{#cite CGGMP21}} in [Canetti's Construction](./threshold-ecdsa-construction/introduction.md). 
- Finally, we briefly specify our implementation and analyse the security and efficiency for the threshold ECDSA construction of Canetti et al for three curves as follows:
    - In [Threhold ECDSA for secp256k1](./intended-implementation/threshold-ecdsa-from-secp256k1.md), we analyse the security and effciency for the threshold ECDSA for the curve **secp256k1**.
    - In  [Threhold ECDSA for ed25519](./intended-implementation/threshold-ecdsa-from-ed25519.md) we analyse the security and effciency for the threshold ECDSA for the curve **ed25519**.
    - In [Threhold ECDSA for curve25519](./intended-implementation/threshold-ecdsa-from-sr-25519.md) we analyse the security and effciency for the threshold ECDSA for the curve **curve25519**.
