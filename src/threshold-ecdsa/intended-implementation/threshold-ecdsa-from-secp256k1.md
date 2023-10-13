
### Threshold ECDSA for Secp256k1

 Bitcoin, the first and most well-known cryptocurrency, relies on **secp256k1** for its public key cryptography. Specifically, it uses the ECDSA algorithm with secp256k1 as the underlying elliptic curve. Many other cryptocurrencies, including Ethereum have since adopted this curve for their digital signature schemes. Since the curve can be used for the orinary ECDSA algorithm, it can also be used for the threshold ECDSA version as well. Below we describe the curve parameter and its security and efficiency analysis.

 The secp256k1 curve parameters \(E:y^2=x^3+ax+b\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:
 - \(p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f\)

 - \(a=0x00\)

 - \(b=0x07\)

 - \(n=0xfffffffffffffffffffffffffffffffebaaedce6af48a03Bbfd25e8cd0364141\)

 - \(g=(0x79be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798, 0x483ada7726a3c4655da4fbfc0e1108a8fd17b448a68554199c47d08ffb10d4b8\)

 For security analysis, this curve is chosen by Satoshi Nakamoto because unlike the popular NIST curves, secp256k1's constants were selected in a predictable way, which significantly reduces the possibility that the curve's creator inserted any sort of backdoor into the curve. In addition, it is implied in {{#cite SECG1}} that the curve has a security level of \(128\) bits, which is considered secure.
 
 For efficiency analysis, the curve a Koblitz curve, a special class of elliptic curves that enables efficient computation. The primary operations of the curve can be done 30% faster than other curves if the implementation is sufficiently optimized
We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations, which has been tested extensively and undergone thorough optimitation, making it very fast to produce signatures. 