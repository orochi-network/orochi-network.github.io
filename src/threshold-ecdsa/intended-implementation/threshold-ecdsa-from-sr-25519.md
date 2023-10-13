
### Threshold signature for sr25519 parameters


The term **sr25519** refers to the instatiation of Schnorr signature using the curve **curve25519**, which is specified in [Schnorrkel](https://github.com/w3f/schnorrkel). Below we describe the curve parameter and its security and efficiency analysis.

The curve parameters \\(E:by^2=x^3+ax^2+x\\) defined over \\(\mathbb{F}_p\\) with order \\(n\\), cofactor \\(f\\) and base point \\(G\\) are as follows:

- \\(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\\)

- \\(a=0x76d06\\)

- \\(b=0x01\\)

- \\(n=0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\\)

- \\(cofactor=1\\)

- \\(G=(0x09,0x20ae19a1b8a086b4e01edd2c7748d14c923d4d7e6d7c61b229e9c5a27eced3d9)\\)

 For security analysis, according to [this link](./http://safecurves.cr.yp.to/rho.html), the curve achieves a \\(126\\) bit security level, which is just a bit less than secp256k1 and ed25519, but still considered secure. 
 
As **sr25519 is actually the Schnorr signature instatiatied with the  curve25519**, we can instatiate the FROST threshold signature scheme of {{#cite KG20}} with the parameters of sr25519 described in  to achieve the threshold version of sr25519.