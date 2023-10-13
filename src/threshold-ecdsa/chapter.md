# Threshold Signature

In this chapter, we give an overview of threshold signatures and describe the threshold ECDSA construction of Canetti et al in {{#cite CGGMP21}} and the FROST threshold signature in {{#cite KG20}}. The chapter is separated into \\(4\\) major parts below:
- First, we give a brief introduction to threshold signatures and state its syntax and security properties in [Introduction](./threshold-ecdsa-introduction/introduction.md). 

- Second, we describe the threshold ECDSA construction of {{#cite CGGMP21}} in [Canetti's Construction](./threshold-ecdsa-construction/introduction.md) to support the threshold ECDSA version for the curve **secp256k1**. 

- Third, we describe the FROST threshold signature construction of {{#cite KG20}} in [FROST Construction](./frost-construction/introduction.md) to support threshold version of **ed25519** using probabilistic nonce generation and **sr25519**.

- Finally, we briefly specify our implementation and analyse the security for the threshold ECDSA construction of Canetti et al using  **secp256k1** parameters and FROST threshold signature construction using **ed25519** and **sr25519** parameters as follows:

    - In [Threhold signature for secp256k1 parameters](./intended-implementation/threshold-ecdsa-from-secp256k1.md), we analyse the security for the threshold signature using the parameters of **secp256k1**.

    - In  [Threhold signature for ed25519 parameters](./intended-implementation/threshold-ecdsa-from-ed25519.md) we analyse the security for the threshold signature using the parameters of **ed25519** using probabilistic nonce generation.

    - In [Threhold signature for sr25519 parameters](./intended-implementation/threshold-ecdsa-from-sr-25519.md) we analyse the security for the threshold signature using the parameters of **sr25519**.
