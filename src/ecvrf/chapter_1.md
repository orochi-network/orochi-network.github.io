# Chapter 1
## Verifiable Random Function (VRF)
### Introduction
In cryptography, a verifiable random function (VRF) is a public key version of a pseudorandom function. It produces a pseudorandom output and a proof certifying that the output is computed correctly. 

A VRF includes a pair of key, named secret and public keys. The secret key, along with the input is used by the holder to compute the value of a VRF and its proof, while the public key is used by anyone to verify the correctness of the computation.

The issue of traditional pseudorandom functions is that their output cannot be verified without the knowledge of the seed. Thus a malicious adversary can choose an output that benefits him and claim that it is the output of the function. VRF solves this by introducing a public key and proofs that can be verified publicly, yet no information about the secret key can be found, while the owner can keep secret key to produce numbers indistinguishable from randomly chosen ones.

VRF has applications in various aspects. Among them, in internet security, it is used to provide privacy against offline enumeration (e.g. dictionary attacks) on data stored in a hash-based data structure [irtf-vrf08](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08). 

### VRF Algorithms
A Verifiable Random Function (VRF) consists of three algorithms \\( (\mathsf{Gen}, \mathsf{Eval}, \mathsf{Verify})\\) where:

**\\((sk,pk) \leftarrow \mathsf{Gen}(1^{\lambda})\\):** This algorithm takes as input a security parameter \\( \lambda \\) and
outputs a key pair \\( (sk,pk)\\).

**\\( (Y,\pi) \leftarrow \mathsf{Eval}(X,sk)\\):** This algorithm takes as input a secret key \\(sk\\) and a value \\(X\\)
and outputs a value \\(Y \in {0,1}^{out(\lambda)} \\) and a proof \\( \pi \\).

**\\( b \leftarrow \mathsf{Verify}(pk,X,Y,\pi)\\):** This algorithm takes an input a public key \\(pk \\), a value \\(X\\), a value \\(Y\\), a proof \\(\pi\\) and outputs a bit \\(b\\) that determines whether \\(Y=\mathsf{Eval}(X,sk)\\).

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

### VRF Based on Elliptic Curves (ECVRF)
We describe a VRF Based on Elliptic Curves in the paper of  [PWHVNRG17]. The Internet Research Task Force (IRTF) also describes this VRF in their Internet-Draft [irtf-vrf08]. The security proof of the VRF is in [PWHVNRG17], we do not present it here.


**Why using ECVRF**

There are many VRF constructions, as listed in the history of VRF. However, ECVRF is chosen because its efficient in both proving and verification complexity, and can be make distributed. 

For example, in the construction of Hohenberger and Waters [HW10], the proof of the VRF contains \\(O(\lambda)\\) group elements. The number of evaluation steps is \\(O(\lambda)\\), because we need to compute \\(O(\lambda)\\) group elements. Verification require \\(O(\lambda)\\) computations using bilinear map, where \\(\lambda\\) is the security parameter. It is unknown whether this VRF can be made distributed or not.

In the other hand, the proof size and the number of evaluation and verification steps of ECVRF are all constant and can be make distributed, as in the paper of Galindo et al [GLOW20]. The VRF construction of [DY05] also have constant proof size, evaluation and verification steps, and can be make distributed. However, in their paper, they require a cyclic group whose order is a 1000 bit prime, while ECVRF require a cyclic group whose order is a 256 bit prime such that the Decisional Diffie-Hellman(DDH) assumption holds. Hence, we can implement VRF using Secp256k1, a curve used by Ethereum for creating digital signature. Because the embeeding degree of Secp256k1 is large, thus the DDH assumption is believed to be true.

**Preliminaries**

Let \\(\mathbb{G}\\) be a cyclic group of prime order \\(p\\) with generator \\(g\\). Denote \\(Z_p\\) to be the set of integers modulo \\(p\\). Let \\(HashToCurve\\) be a hash function mapping a bit string to an element in \\(\mathbb{G}\\). Let \\(HashPoints\\) be a hash function mapping arbitary input length to a \\(256\\) bit integer.

Note that, in the paper of [PWHVNRG17], the functions \\(HashToCurve\\) and \\(HashPoint\\) are modeled as random oracle model. This is used to prove the security of the VRF. 

Since we use the curve Secp256k1, the cofactor parameter is set to \\(1\\).


**ECVRF Construction**

**\\(KeyGen(1^{k})\\):** Choose a random secret value \\(sk \in Z_p\\) and the secret key is set to be \\(sk \\). The public key is  \\(pk=g^{sk}\\).

**\\(Eval(sk,X)\\):** Given the secret key \\(sk\\) and an input \\(X\\), the pseudorandom value \\(Y\\) and its proof \\(\pi\\) is computed as follow:

Compute \\(h=HashToCurve(X,pk)\\).

Compute \\(\gamma=h^{sk}\\).

Choose a value \\(k\\) uniformly in \\(Z_p\\).

Compute \\(c=HashPoint(g,h,pk,gamma,g^k,h^k)\\)

Compute \\(s \equiv k-c.sk \pmod{q}\\)

Compute \\(gammastr=PointToString(\gamma)\\)

The output Y of the VRF is computed as \\(Y=SHA256(gammastr)\\)

The proof \\(\pi\\) of the VRF is computed as \\(\pi=(\gamma,c,s)\\) 

**\\(Verify(pk,X,Y,\pi)\\):** Given the public key \\(pk\\), the VRF input \\(X\\), the VRF output \\(Y\\) and its proof \\(\pi=(\gamma,c,s)\\), the verification step proceed as follow:

Compute \\(u=pk^cg^s\\)

Compute \\(h=HashToCurve(X)\\)

Compute \\(v=\gamma^ch^s\\)

Check if \\(c=HashPoint(g,h,pk,gamma,g^k,h^k)\\) and \\(Y=ProofToHash(\gamma)\\)

**ECVRF Auxiliary Functions**

 In this section, we describe the construction of \\(HashToCurve\\) and \\(HashPoint\\) in the Internet-Draft of irtf. More details can be found in [irtf-vrf08]. 

 **\\(HashToCurve(X,pk)\\):** Given two group elements \\(X, pk \in \mathbb{G}\\), the function output a hash value in \\(Z_p\\) as follow:

Let \\(ctr=0\\)

Compute \\(pkstr=PointToString(pk)\\)

Define \\(H\\) to be **"INVALID"**

While \\(H\\) is **"INVALID"** or \\(H\\) is the identity element of the group:

- Compute \\(ctrstr=IntToString(ctr)\\)

- Compute \\(hstr=SHA256\\)( "0x01" || "0x01" || \\(pkstr\\) || \\(X\\) || \\(ctrstr\\) || "0x00")

- Compute \\(H\\)=\\(StringToPoint(hstr)\\)

- Increment \\(ctr\\) by \\(1\\)

Output \\(H\\).

 **\\(HashPoint(P_1,P_2,...,P_n)\\):** Given n elements in  \\(\mathbb{G}\\), the hash value is computed as follow:

Initialize \\(str=\\)""

For \\(i=1,2,...,n\\):

- Update \\(str= str || PointToString(P_i)\\)
    
Update \\(str=SHA256(str)\\)

Compute \\(c=StringToInt(str)\\)

Output \\(c\\)

The function \\(PointToString\\) converts a point of an elliptic curve to a string. It is specified in section 2.3.3 of [SECG1]

The function \\(StringToPoint\\) converts a string to a point of an elliptic curve. It is specified in section 2.3.4 of [SECG1]

## Implelemtation

We implement the VRF in Rust using the library libsecp256k1.

### References

[MRV99] Micali, S., Rabin, M., & Vadhan, S. (1999, October). Verifiable random functions. In 40th annual symposium on foundations of computer science (cat. No. 99CB37039) (pp. 120-130). IEEE.

[Lys02] Lysyanskaya, A. (2002, August). Unique signatures and verifiable random functions from the DH-DDH separation. In Annual International Cryptology Conference (pp. 597-612). Springer, Berlin, Heidelberg.

[DY05] Dodis, Y., & Yampolskiy, A. (2005, January). A verifiable random function with short proofs and keys. In International Workshop on Public Key Cryptography (pp. 416-431). Springer, Berlin, Heidelberg.

[HW10] Hohenberger, S., & Waters, B. (2010, May). Constructing verifiable random functions with large input spaces. In Annual International Conference on the Theory and Applications of Cryptographic Techniques (pp. 656-672). Springer, Berlin, Heidelberg.

[BMR10] Boneh, D., Montgomery, H. W., & Raghunathan, A. (2010, October). Algebraic pseudorandom functions with improved efficiency from the augmented cascade. In Proceedings of the 17th ACM conference on Computer and communications security (pp. 131-140).

[Jag15] Jager, T. (2015, March). Verifiable random functions from weaker assumptions. In Theory of Cryptography Conference (pp. 121-143). Springer, Berlin, Heidelberg.

[HJ15] Hofheinz, D., & Jager, T. (2016, January). Verifiable random functions from standard assumptions. In Theory of Cryptography Conference (pp. 336-362). Springer, Berlin, Heidelberg. 

[PWHVNRG17] Papadopoulos, D., Wessels, D., Huque, S., Naor, M., Včelák, J., Reyzin, L., & Goldberg, S. (2017). Making NSEC5 practical for DNSSEC. Cryptology ePrint Archive.

[EKSSZSC20] Esgin, M. F., Kuchta, V., Sakzad, A., Steinfeld, R., Zhang, Z., Sun, S., & Chu, S. (2021, March). Practical post-quantum few-time verifiable random function with applications to algorand. In International Conference on Financial Cryptography and Data Security (pp. 560-578). Springer, Berlin, Heidelberg.

[GLOW20] Galindo, D., Liu, J., Ordean, M., & Wong, J. M. (2021, September). Fully distributed verifiable random functions and their application to decentralised random beacons. In 2021 IEEE European Symposium on Security and Privacy (EuroS&P) (pp. 88-102). IEEE.

[irtf-vrf08] https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08

[SECG1]  Standards for Efficient Cryptography Group (SECG), "SEC 1: Elliptic Curve Cryptography", Version 2.0, May 2009, <http://www.secg.org/sec1-v2.pdf>.

[algorand] https://medium.com/algorand/algorand-releases-first-open-source-code-of-verifiable-random-function-93c2960abd61

[polkadot] https://wiki.polkadot.network/docs/learn-randomness

[chainlink] https://blog.chain.link/verifiable-random-function-vrf/
