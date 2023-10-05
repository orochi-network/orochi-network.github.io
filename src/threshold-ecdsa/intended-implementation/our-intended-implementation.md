##  Implementation Guideline
In this section, we are going to briefly our implementation. First, we will specify our choice of language and the supported curves of our implementation as well as their supporting libraries. Then we will discuss 
some security concerns when implementing the protocol.

### Programming Language: 
We intend to implement the threshold ECDSA protocol by the Rust programming language. Althought Rust is a relatively young programming language, it has numerous advantage. Due to its strict rules of memory management, Rust delivers safe code. Rust compiler produces efficient output optimized for performance while preserving the ability to access the data at a very low level, similar to what C/C++ does. Hence, we choose Rust because of the abovementioned properties.


### Security Consideration

Finally, we would like to discuss several concerns that should be noted when implementing the threshold ECDSA protocol. 

- **Proving the Discrete Log Relation in Composite Modulus:** Recall that in Step 3 of the key refresh process, we require the participants \\(P_i\\) to prove the discrete log relation between \\(h_{i1}\\) and \\(h_{i2}\\) in modulo \\(N_i\\). This can be done using Schnorr's protocol for Ring, as specified in (see [Supporting Protocols](./supporting-algorithms.md)). The protocol uses binary challenge \\(c \in \\{0,1\\}\\), which has soundness error \\(1/2\\). It is repeated \\(\lambda\\) times to achieve soundness error \\(1/2^\lambda\\). When the order of the group is a prime number \\(p\\), one can **extend the challenge set** to be \\(\mathbb{Z}_p\\) and execute the protocol **only once**, this is the case of an ordinary Schnorr protocol. However, we cannot do the same thing for the group \\(\mathbb{Z}^*_N\\), since its order is divisible by \\(2\\). [Verichain](https://www.verichains.io/tsshock/) showed that doing so can leak the secret key of the protocol. Hence, we need to **repeat the Schnorr protocol for Ring** \\(\lambda\\) **times when proving the discrete log relation in modulo** \\(N_i\\) for each \\(i\\).

- **The Requirement of Range Proof:** Recall that in step 2 of the signing protocol, we require that each participant has to prove that some of their secret range must lie in a specified value (we require \\(k_i,\lambda_i,w_i, \le 2^{3\lambda}\\) and \\(\gamma_{ij},v_{ij}<2^{7\lambda}\\)). The range proof might looks unnecessary at the first glance, however it is shown in {{#cite TS21}} that the lack of these range proofs can break the security of the protocol. The reason for this is that range proofs ensure that the encrypted values are within a specified range, and prevent attackers from tampering with the values or sending invalid data. Hence it is necessary to **ensure that these range proofs are done** in the protocol.

