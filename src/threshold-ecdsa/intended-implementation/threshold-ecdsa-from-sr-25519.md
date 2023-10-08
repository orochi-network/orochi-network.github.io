
### Threshold ECDSA for Sr25519

- **sr25519:** This curve is implemented in the [Schnorrkel](https://github.com/w3f/schnorrkel) library to support Schnorr sinature. The curve parameters \(E:by^2=x^3+ax^2+x\) defined over \(\mathbb{F}_p\) are as follows:

- \(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\)
- \(a=0x76d06\)
-  \(b=0x01\)


Technically, the curve can also be used in threshold ECDSA, hence we intend to support the choice of sr25519 in our implementation version.