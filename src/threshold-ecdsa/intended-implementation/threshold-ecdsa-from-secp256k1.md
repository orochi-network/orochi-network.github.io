
### Threshold ECDSA for Secp256k1

 - **secp256k1:** This is the curve used by Bitcoin as well as Ethereum for signing, it is also the first curve we will support in our implementation.
 The curve parameters \(E:y^2=x^3+ax+b\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:
 - \(p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f\)
 - \(a=0x00\)
 - \(b=0x07\)
 - \(n=0xfffffffffffffffffffffffffffffffebaaedce6af48a03Bbfd25e8cd0364141\)

 For security and efficiency analysis, this curve is chosen by Satoshi Nakamoto because the other curves recommended by NIST might contain backdoor. Althought  it is not a safe curve,
  We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations.