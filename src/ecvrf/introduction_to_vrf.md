# Chapter 1
## Introduction to Verifiable Random Function (VRF)
### Introduction
In cryptography, a verifiable random function (VRF) is a public key version of a pseudorandom function. It produces a pseudorandom output and a proof certifying that the output is computed correctly. 

A VRF includes a pair of key, named secret public keys. The secret key, along with the input is used by the holder to compute the value of a VRF and its proof, while the public key is used by anyone to verify the correctness of the computation.

The issue with traditional pseudorandom functions is that their output cannot be verified without the knowledge of the seed. Thus a malicious adversary can choose an output that benefits him and claim that it is the output of the function. VRF solve this by introducing a public key and proofs that can be verified publicly, yet no informations about the secret key can be found, while the owner can keep secret key to produce numbers indistinguishable from randomly chosen ones.

VRF has applications in various aspects. Among them, in Internet security, it is used to provide privacy against offline enumeration (e.g. dictionary attacks) on data stored in a hash-based data structure [irtf-vrf08](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08). VRF is also used in lottery systems [] and e cashes [BCKL09].

### VRF Algorithms
A Verifiable Random Function consists of three algorithms \\( (\mathsf{Gen}, \mathsf{Eval}, \mathsf{Verify})\\) where:

**\\((sk,pk) \leftarrow \mathsf{Gen}(1^{\lambda})\\):** This algorithm takes an input as a security parameter \\( \lambda \\) and
outputs a key pair \\( (sk,pk)\\).

**\\( (Y,\pi) \leftarrow \mathsf{Eval}(X,sk)\\):** This algorithm takes as input a secret key \\(sk\\) and a value \\(X\\)
and outputs a value \\(Y \in {0,1}^{out(\lambda)} \\) and a proof \\( \pi \\).

**\\( b \leftarrow \mathsf{Verify}(pk,X,Y,\pi)\\):** This algorithm takes an input a public key \\(pk \\), a value \\(X\\), a value \\(Y\\), a proof \\(\pi\\) and outputs a bit \\(b\\) that determines whether \\(Y=Eval(X,sk)\\).

### VRF Security Propeties
We need a VRF to have the following properties:

**Correctness:** If \\((Y,\pi)=Eval(sk,X)\\) then \\(Verify(pk,X,Y,\pi)=1\\)

**Uniqueness:** There does not exists tuples \\((Y,\pi)\\) and \\(Y',\pi'\\) with \\(Y \ne Y'\\) and:
\\(Verify(pk,X,Y,\pi)=Verify(pk,X,Y',\pi')=1\\)

**Pseudorandom:** For any adversary \\(\mathcal{A}\\) the probability \\(|Pr[ExpRand^A_{VRF}(\lambda)=1]-\dfrac{1}{2}|\\) is negilible where \\(ExpRand^A_{VRF}(\lambda)\\) is defined as follow:

\\(ExpRand^A_{VRF}(\lambda)\\):

- \\((sk,pk) \leftarrow Gen(1^{\lambda})\\)
- \\((X^*,st) \leftarrow \mathcal{A}^{\mathcal{O_{VRF}}(.)}(pk)\\)
- \\(Y_0 \leftarrow Eval(X*,sk)\\)
- \\(Y_1 \leftarrow {0,1}^{out(\lambda)}\\)
- \\({0,1} \leftarrow b\\)
- \\(b' \leftarrow \mathcal{A}(Y_b,st)\\)
- Return \\(b=b'\\)

In the paper of [PWHVNRG17], the authors stated that a VRF must also be collision resistant. This property is formally defined below:

**Collision Resistant:** For any adversarial prover \\(\mathcal{A}=(\mathcal{A_1},\mathcal{A_2})\\) the probability \\(|Pr[ExpCol^A_{VRF}(\lambda)=1]\\) is negilible where \\(ExpCol^A_{VRF}(\lambda)\\) is defined as follow:

\\(ExpCol^A_{VRF}(\lambda)\\):

- \\((sk,pk) \leftarrow \mathcal{A_1}(Gen(1^{\lambda}))\\)
- \\((X,X',st) \leftarrow \mathcal{A_2}(sk,pk)\\)
- Return \\(X \ne X'\\) and \\(Eval(X,sk)=Eval(X',sk)\\)


It is interesting to see that, VRF can be used for signing messages. However, several signing algorithm such as ECDSA cannot be used to build a VRF. For a given message and a secret key, there can be multiple valid signatures, thus an adversiral prover could produce different valid outputs from a given input, and choose the one that benefits him. This contradict the uniqueness property of VRF. 


### History of Verifiable Random Function
Verifiable Random Function is introduced by Micali, Rabin and Vadhan [MRV99]. They defined a Verifiable Random Function (VUF) and gave a construction based on the RSA assumption [MRV99], then proved that a VRF can be constructed from a VUF [MRV99]. 

In 2002, Lysyanskaya [Lys02] also followed the same path by constructing a VUF instead VRF. However, Lysyanskaya's VUF is based on Diffie-Hellman assumption instead, and it is the first construction that is based on Diffie-Hellman assumption. 

In 2005, Dodis and Yampolsky [DY05] gave a direct and efficient construction using bilinear map, then improved. Later, during the 2010s, many constructions [HW10,BMR10,Jag15] also used bilinear map, all of them used non standard assumptions to prove the security of their VRF. 

In 2015, Hofheinz and Jager [HJ15] constructed a VRF that is based on a constant-size complexity assumption, namely the \\((n-1)\\)-linear assumption. The \\((n-1)\\)-linear assumption is not easier to break as \\(n\\) grows larger, as opposed to all the assumptions used by previous constructions [HS07].

In 2017, [PWHVNRG17] construct an efficient VRF based on elliptic curve that does not use bilinear map to produce output and verify, instead they only use hash functions and elliptic curve operations. In the other hand, their hash function is viewed as random oracle model in the construction.

In 2019, Nir Bitansky [Bit19] showed that VRF can be constructed from non-interactive witness-indistinguishable proof (NIWI).

In 2020, Esgin et al [EKSSZSC20] was the first to construct a VRF based on lattice based cryptography, which is resistant to attack from quantum computers.

### VRF in Blockchain

Many blockchains, namely Algorand [algorand], Carnado [carnado], Polkadot [polkadot] use VRF to randomly select the next block proposer.

In 2020, Chainlink use VRF as a secure source of on-chain randomness across the Web3 ecosystem, including in leading GameFi, DeFi, and NFT projects.[chainlink]

In 2021, Harmony use VRF as a source of randomness for their sharding.


### References

[MR02] Silvio Micali and Ronald L. Rivest. “Micropayments Revisited”. In: Proc. CT-RSA 2002. Vol. 2271. Lecture Notes in Computer Science. Springer, 2002, pp. 149–163.

[BCKL09] Mira Belenkiy, Melissa Chase, Markulf Kohlweiss, and Anna Lysyanskaya. “Compact E-Cash and Simulatable VRFs Revisited”. In: Proc. Pairing 2009. Vol. 5671. Lecture Notes in Computer Science. Springer, 2009, pp. 114–131.

[MRV99] Micali, S., Rabin, M., & Vadhan, S. (1999, October). Verifiable random functions. In 40th annual symposium on foundations of computer science (cat. No. 99CB37039) (pp. 120-130). IEEE.

[Lys02] Lysyanskaya, A. (2002, August). Unique signatures and verifiable random functions from the DH-DDH separation. In Annual International Cryptology Conference (pp. 597-612). Springer, Berlin, Heidelberg.

[DY05] Dodis, Y., & Yampolskiy, A. (2005, January). A verifiable random function with short proofs and keys. In International Workshop on Public Key Cryptography (pp. 416-431). Springer, Berlin, Heidelberg.

[HW10] Hohenberger, S., & Waters, B. (2010, May). Constructing verifiable random functions with large input spaces. In Annual International Conference on the Theory and Applications of Cryptographic Techniques (pp. 656-672). Springer, Berlin, Heidelberg.

[BMR10] Boneh, D., Montgomery, H. W., & Raghunathan, A. (2010, October). Algebraic pseudorandom functions with improved efficiency from the augmented cascade. In Proceedings of the 17th ACM conference on Computer and communications security (pp. 131-140).

[Jag15] Jager, T. (2015, March). Verifiable random functions from weaker assumptions. In Theory of Cryptography Conference (pp. 121-143). Springer, Berlin, Heidelberg.

[HJ15] Hofheinz, D., & Jager, T. (2016, January). Verifiable random functions from standard assumptions. In Theory of Cryptography Conference (pp. 336-362). Springer, Berlin, Heidelberg. 

[PWHVNRG17] Papadopoulos, D., Wessels, D., Huque, S., Naor, M., Včelák, J., Reyzin, L., & Goldberg, S. (2017). Making NSEC5 practical for DNSSEC. Cryptology ePrint Archive.

[EKSSZSC20] Esgin, M. F., Kuchta, V., Sakzad, A., Steinfeld, R., Zhang, Z., Sun, S., & Chu, S. (2021, March). Practical post-quantum few-time verifiable random function with applications to algorand. In International Conference on Financial Cryptography and Data Security (pp. 560-578). Springer, Berlin, Heidelberg.

[irtf-vrf08] https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08

[algorand] https://medium.com/algorand/algorand-releases-first-open-source-code-of-verifiable-random-function-93c2960abd61

[polkadot] https://wiki.polkadot.network/docs/learn-randomness

[chainlink] https://blog.chain.link/verifiable-random-function-vrf/
