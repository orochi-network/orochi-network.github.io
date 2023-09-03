##  Implementation Specification
In this section, we are going to specify our implementation. We are going to specify our choice of language and the supported curves of our implementation as well as their supporting libraries.


**Programming Language:** We intend to implement the threshold ECDSA protocol by the Rust programming language. Althought Rust is a relatively young programming language, it has numerous advantage. Due to its strict rules of memory management, Rust delivers safe code. Rust compiler produces efficient output optimized for performance while preserving the ability to access the data at a very low level, similar to what C/C++ does. Hence, we choose Rust because of the abovementioned properties.

**The Choice of Elliptic Curves:** First, we need to specify what curve are we going to use. Recall that the security lies in the discrete log assumption, hence it sufficies to choose any curve where the discrete log assumption is hard. The three curves supported in our implementation are follow:
 - **secp256k1:** This is the curve used by Bitcoin as well as Ethereum for signing, it is also the first curve we will support in our implementation. We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations.
 - **edd25519:** The curve edd25519 is standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). The curve is popular due to its high speed compared to other curves without sacrificing security. Thus, our implelemtation also intend to support this curve. 
 - **sr25519:** This curve is implemented in the [Schnorrkel](https://github.com/w3f/schnorrkel) library to support Schnorr sinature. Technically, the curve can also be used in threshold ECDSA, hence we intend to support the choice of sr25519 in our implementation version.

**The Choice of Hash Function:** Finally, the commitment scheme \\(\mathsf{Com}\\) used in the signing protocol involves using a hash function in practice, as mentioned in [Supporting Protocols](./supporting-algorithms.md). In our implementation, we intend to use the **keccak** hash function.