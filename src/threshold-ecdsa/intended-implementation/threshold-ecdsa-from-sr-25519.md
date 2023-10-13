
### Threshold signature for sr25519


The term **sr25519** refers to the instatiation of Schnorr signature using the curve **curve25519**, the same curve as of EDDSA, which is specified in [Schnorrkel](https://github.com/w3f/schnorrkel). However, it additionally employs the method of point compression due to  [Ristretto](https://ristretto.group) to make makes Schnorr signatures over the Edward's curve more secure. Below we describe the curve parameter and its security and efficiency analysis.

The **sr25519** scheme supports both forms of **curve25519**, i.e, its twisted Edward form and Montomery form. The twisted Edward form of **curve25519** has been described in the previous Section. For the Montomery form, the curve can be written as \\(E:by^2=x^3+ax^2+x\\) over \\(\mathbb{F}_p\\) with order \\(n\\), cofactor \\(f\\) and base point \\(G\\) are as follows:

- \\(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\\)

- \\(a=0x76d06\\)

- \\(b=0x01\\)

- \\(n=0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\\)

- \\(cofactor=1\\)

- \\(G=(0x09,0x20ae19a1b8a086b4e01edd2c7748d14c923d4d7e6d7c61b229e9c5a27eced3d9)\\)

 For security analysis, recall that the curve **curve25519** achieves \\(128\\) bit security level, as specified in the previous Section. However, since **sr25519** additionally uses the point compression due to Ristretto, it is safe from the bug could lead to a double spend expoit of [Monero](./https://www.getmonero.org/2017/05/17/disclosure-of-a-major-bug-in-cryptonote-based-currencies.html)
 
Finally, because **sr25519 is actually the Schnorr signature instatiatied with the  curve25519** and FROST is a threshold Schnorr signature scheme that can be instatiated by any curve where the discrete log problem is hard, we can instatiate the FROST threshold signature scheme of {{#cite KG20}} with the parameters of **sr25519** described in  to achieve the threshold version of **sr25519**.