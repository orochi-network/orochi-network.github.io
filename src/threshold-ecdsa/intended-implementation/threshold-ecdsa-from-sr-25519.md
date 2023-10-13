
### Threshold ECDSA for Sr25519


The term **sr25519** refers to the instatiatied of Schnorr signature using the curve **curve25519**, which is specified in [Schnorrkel](https://github.com/w3f/schnorrkel). However, as threshold ECDSA can be used on any curve where the discrete log algorithm is hard, the curve **curve25519** can also be used in the construction. The curve parameters \(E:by^2=x^3+ax^2+x\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:

- \(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\)

- \(a=0x76d06\)

- \(b=0x01\)

- \(n=0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\)

- \(g=(0x09,)\)

 For security analysis, according to [this link](./http://safecurves.cr.yp.to/rho.html), the curve achieves a \(126\) bit security level, which is just a bit less than secp256k1 and ed25519, but still considered secure. 

 For efficiency analysis, 
 
 Again, recall that the curve curve25519 is developed to support Schnorr signature, in Canneti's construction, **we only require any group where the discrete log algorithm is hard**. Since the curve25519 group above achieves \(128\) bit security level, **we can instatiate the theshold signature construction of Canneti et al. using the curve25519 curve specified above**. Hence we intend to support the choice of sr25519 in our implementation version.