##  Implementation Guideline
In this section, we are going to briefly our implementation. First, we will specify our choice of language and the supported curves of our implementation as well as their supporting libraries. Then we will discuss 
some security concerns when implementing the protocol.

### Programming Language: 
We intend to implement the threshold ECDSA protocol by the Rust programming language. Althought Rust is a relatively young programming language, it has numerous advantage. Due to its strict rules of memory management, Rust delivers safe code. Rust compiler produces efficient output optimized for performance while preserving the ability to access the data at a very low level, similar to what C/C++ does. Hence, we choose Rust because of the abovementioned properties.

### The Choice of Elliptic Curves:

We now specify what curve are we going to use in our implementation. Recall that the security lies in the discrete log assumption, hence it sufficies to choose any curve where the discrete log assumption is hard. The three curves supported in our implementation are follow:
 - **secp256k1:** This is the curve used by Bitcoin as well as Ethereum for signing, it is also the first curve we will support in our implementation. We intend to use the library [libsecp256k1](https://github.com/paritytech/libsecp256k1) for curve operations.
 - **edd25519:** The curve edd25519 is standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). The curve is popular due to its high speed compared to other curves without sacrificing security. Thus, our implelemtation also intend to support this curve. 
 - **sr25519:** This curve is implemented in the [Schnorrkel](https://github.com/w3f/schnorrkel) library to support Schnorr sinature. Technically, the curve can also be used in threshold ECDSA, hence we intend to support the choice of sr25519 in our implementation version.

### The Choice of Hash Function

Recall that the commitment scheme \\(\mathsf{Com}\\) used in the signing protocol involves using a hash function in practice, as mentioned in [Supporting Protocols](./supporting-algorithms.md). In our implementation, we intend to use the **keccak** hash function.


### Security Consideration

Finally, we would like to discuss several concerns that should be noted when implementing the threshold ECDSA protocol. 

- **Proving the Discrete Log Relation in Composite Modulus:** Recall that in Step 3 of the Key Generation process, we require the participants \\(P_i\\) to prove the discrete log relation between \\(h_{i1}\\) and \\(h_{i2}\\) in modulo \\(N_i\\). This can be done using Schnorr's protocol {{#cite S91}}. Initially, Schnorr's protocol use binary challenge \\(c \in \\{0,1\\}\\), which has soundness error \\(1/2\\). The protocol is repeated \\(\lambda\\) times to achieve soundness error \\(1/2^\lambda\\). For groups of prime order \\(p\\), one can **extend the challenge set** to be \\(\mathbb{Z}_p\\) and execute the protocol **only once** (see [Supporting Protocols](./supporting-algorithms.md)). However, we cannot do the same thing for the group \\(\mathbb{Z}^*_N\\), since its order is divisible by \\(2\\). [Verichain](https://www.verichains.io/tsshock/) showed that doing so can leak the secret key of the protocol. Hence, we need to **repeat the Schnorr protocol** \\(\lambda\\) **times when proving the discrete log relation in modulo** \\(N_i\\) for each \\(i\\).

- **The Requirement of Range Proof:** Recall that in the \\(\mathsf{MtA}\\) protocol, we require that each participant has to prove that some of their secret range must lie in a specified value \\((a,b \le p^3~\text{and}~\beta_2 \le p^7)\\). The range proof might looks unnecessary at the first glance, however it is shown in {{#cite TS21}} that the lack of these range proofs can break the security of the protocol. The reason for this is that range proofs ensure that the encrypted values are within a specified range, and prevent attackers from tampering with the values or sending invalid data. Hence it is necessary to **ensure that these range proofs are done** in the protocol.

- **Ensure that N is a Product of Two Safe Primes:** In the paper of {{#GG20}}, the authors only required each participant to prove that the modulus \\(N\\) of the cryptosystem to be **squarefree** instead of proving it to be the product of two prime factors. However, {{#cite MY23}} showed that it is possible to break the security of the protocol by allowing a malicious participant to select a squarefree \\(N\\) having 16 prime factors. Fortunately, the abovementioned problem can be fixed by **combining all the protocols in** {{#cite GMR98}} **to properly prove that** \\(N\\) **is a product of two safe primes**.