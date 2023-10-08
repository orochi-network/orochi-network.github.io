
### Threshold ECDSA for Secp256k1

 - **secp256k1:** This is the curve used by Bitcoin as well as Ethereum for signing, it is also the first curve we will support in our implementation.
 The curve parameters \(E:y^2=x^3+ax+b\) defined over \(\mathbb{F}_p\) are as follows:
 - \(p=0xFFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFE FFFFFC2F\)
 - \(a=0x00\)
 - \(b=0x07\)
 
 For security and efficiency analysis,
  We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations.