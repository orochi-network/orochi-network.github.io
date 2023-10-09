
### Threshold ECDSA for Secp256k1

 This is the curve used by Bitcoin as well as Ethereum for signing, it is also the first curve we will support in our implementation.
 The curve parameters \(E:y^2=x^3+ax+b\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:
 - \(p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f\)
 - \(a=0x00\)
 - \(b=0x07\)
 - \(n=0xfffffffffffffffffffffffffffffffebaaedce6af48a03Bbfd25e8cd0364141\)

 For security analysis, this curve is chosen by Satoshi Nakamoto because unlike the popular NIST curves, secp256k1's constants were selected in a predictable way, which significantly reduces the possibility that the curve's creator inserted any sort of backdoor into the curve. Althought  it is not a safe curve,  . For efficiency analysis, it is often more than 30% faster than other curves if the implementation is sufficiently optimized
  We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations. 