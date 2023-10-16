### Threshold signature for ed25519

**Ed25519** is the most popular instance of the Edwards-curve Digital Signature Algorithm (EdDSA) standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). In the paper, the authors **instatiatied the Schnorr signature scheme with the curve curve25519 in its twisted Edward form** instead of an ordinary elliptic curve such as secp256k1. The used curve in the scheme is popular due to its high speed compared to other curves without sacrificing security. Below we describe the curve parameters and its security and efficiency analysis.

The curve parameters \\(E: ax^2+y^2=1+bx^2y^2\\) defined over \\(\mathbb{F}\_p\\) with order \\(n\\), cofactor \\(f\\) and base point \\(G\\) are as follows:

- \\(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\\)

- \\(a=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffec\\)

- \\(b=0x52036cee2b6ffe738cc740797779e89800700a4d4141d8ab75eb4dca135978a3\\)

- \\(n= 0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\\)

- \\(f=8\\)

- \\(G=(0x216936d3cd6e53fec0a4e231fdd6dc5c692cc7609525a7b2c9562d608f25d51a,\\)
  \\(0x6666666666666666666666666666666666666666666666666666666666666658)\\)

For security analysis, the provable security of the instatiation of ed25519 parameters has been well studied in {{#cite BCJZ20}}. In addition, it has been confirmed in {{#cite BDLSY12}} that the curve achieves \\(128\\) bit security level, the same security level as secp256k1, which is considered secure. However, due to having the cofactor of \\(8\\), the scheme could be potentially vulnerable to a [double spend exploit](./https://www.getmonero.org/2017/05/17/disclosure-of-a-major-bug-in-cryptonote-based-currencies.html).

Recall that **ed25519 is just a variant of Schnorr signature instatiatied with a a twisted Edward curve**, and its signature form \\(R,c,z\\) is identical to an ordinary Schnorr signature scheme, we can instatiate the FROST threshold signature scheme of {{#cite KG20}} with the parameters of **ed25519** described in to achieve the MPC version of **ed25519**.
