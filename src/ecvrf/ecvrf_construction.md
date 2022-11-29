
# Chapter 2

## VRF Based on Elliptic Curves (ECVRF)
We describe a VRF Based on Elliptic Curves in the paper of  [PWHVNRG17]. The Internet Research Task Force (IRTF) also describes this VRF in their Internet-Draft [irtf-vrf08]. The security proof of the VRF is in [PWHVNRG17], we do not present it here.


**Why using ECVRF**

There are many VRF constructions, as listed in the history of VRF. However, ECVRF is chosen because its efficient in both proving and verification complexity, and can be make distributed. 

For example, in the construction of Hohenberger and Waters [HW10], the proof of the VRF contains \\(O(\lambda)\\) group elements. The number of evaluation steps is \\(O(\lambda)\\), because we need to compute \\(O(\lambda)\\) group elements. Verification require \\(O(\lambda)\\) computations using bilinear map, where \\(\lambda\\) is the security parameter. It is unknown whether this VRF can be made distributed or not.

In the other hand, the proof size and the number of evaluation and verification steps of ECVRF are all constant and can be make distributed, as in the paper of Galindo et al [GLOW20]. The VRF construction of [DY05] also have constant proof size, evaluation and verification steps, and can be make distributed. However, in their paper, they require a cyclic group whose order is a 1000 bit prime, while ECVRF require a cyclic group whose order is a 256 bit prime such that the Decisional Diffie-Hellman(DDH) assumption holds. Hence, we can implement VRF using Secp256k1, a curve used by Ethereum for creating digital signature. Because the embeeding degree of Secp256k1 is large, thus the DDH assumption is believed to be true.

**Preliminaries**

Let \\(\mathbb{G}\\) be a cyclic group of prime order \\(p\\) with generator \\(g\\). Denote \\(Z_p\\) to be the set of integers modulo \\(p\\). Let \\(\mathsf{HashToCurve}\\) be a hash function mapping a bit string to an element in \\(\mathbb{G}\\). Let \\(\mathsf{HashPoints}\\) be a hash function mapping arbitary input length to a \\(256\\) bit integer.

Note that, in the paper of [PWHVNRG17], the functions \\(\mathsf{HashToCurve}\\) and \\(\mathsf{HashPoint}\\) are modeled as random oracle model. This is used to prove the security of the VRF. 

Since we use the curve Secp256k1, the cofactor parameter is set to \\(1\\).


**ECVRF Construction**

**\\(\mathsf{KeyGen}(1^{k})\\):** Choose a random secret value \\(sk \in Z_p\\) and the secret key is set to be \\(sk \\). The public key is  \\(pk=g^{sk}\\).

**\\(\mathsf{Eval}(sk,X)\\):** Given the secret key \\(sk\\) and an input \\(X\\), the pseudorandom value \\(Y\\) and its proof \\(\pi\\) is computed as follow:

Compute \\(h=\mathsf{HashToCurve}(X,pk)\\).

Compute \\(\gamma=h^{sk}\\).

Choose a value \\(k\\) uniformly in \\(Z_p\\).

Compute \\(c=\mathsf{HashPoint}(g,h,pk,gamma,g^k,h^k)\\)

Compute \\(s \equiv k-c.sk \pmod{q}\\)

Compute \\(gammastr=\mathsf{PointToString}(\gamma)\\)

The output Y of the VRF is computed as \\(Y=\mathsf{SHA256}(gammastr)\\)

The proof \\(\pi\\) of the VRF is computed as \\(\pi=(\gamma,c,s)\\) 

**\\(\mathsf{Verify}(pk,X,Y,\pi)\\):** Given the public key \\(pk\\), the VRF input \\(X\\), the VRF output \\(Y\\) and its proof \\(\pi=(\gamma,c,s)\\), the verification step proceed as follow:

Compute \\(u=pk^cg^s\\)

Compute \\(h=\mathsf{HashToCurve}(X)\\)

Compute \\(v=\gamma^ch^s\\)

Check if \\(c=\mathsf{HashPoint}(g,h,pk,gamma,g^k,h^k)\\) and \\(Y=\mathsf{ProofToHash}(\gamma)\\)

**ECVRF Auxiliary Functions**

 In this section, we describe the construction of \\(HashToCurve\\) and \\(HashPoint\\) in the Internet-Draft of irtf. More details can be found in [irtf-vrf08]. 

 **\\(\mathsf{HashToCurve}(X,pk)\\):** Given two group elements \\(X, pk \in \mathbb{G}\\), the function output a hash value in \\(Z_p\\) as follow:

Let \\(ctr=0\\)

Compute \\(pkstr=\mathsf{PointToString}(pk)\\)

Define \\(H\\) to be **"INVALID"**

While \\(H\\) is **"INVALID"** or \\(H\\) is the identity element of the group:

- Compute \\(ctrstr=\mathsf{IntToString}(ctr)\\)

- Compute \\(hstr=\mathsf{SHA256}\\)( "0x01" || "0x01" || \\(pkstr\\) || \\(X\\) || \\(ctrstr\\) || "0x00")

- Compute \\(H\\)=\\(\mathsf{StringToPoint}(hstr)\\)

- Increment \\(ctr\\) by \\(1\\)

Output \\(H\\).

 **\\(\mathsf{HashPoint}(P_1,P_2,...,P_n)\\):** Given n elements in  \\(\mathbb{G}\\), the hash value is computed as follow:

Initialize \\(str=\\)""

For \\(i=1,2,...,n\\):

- Update \\(str= str || \mathsf{PointToString}(P_i)\\)
    
Update \\(str=\mathsf{SHA256}(str)\\)

Compute \\(c=\mathsf{StringToInt}(str)\\)

Output \\(c\\)

The function \\(\mathsf{PointToString}\\) converts a point of an elliptic curve to a string. It is specified in section 2.3.3 of [SECG1]

The function \\(\mathsf{StringToPoint}\\) converts a string to a point of an elliptic curve. It is specified in section 2.3.4 of [SECG1]

## Implelemtation

We implement the VRF in Rust using the library libsecp256k1.

### References







[DY05] Dodis, Y., & Yampolskiy, A. (2005, January). A verifiable random function with short proofs and keys. In International Workshop on Public Key Cryptography (pp. 416-431). Springer, Berlin, Heidelberg.

[HW10] Hohenberger, S., & Waters, B. (2010, May). Constructing verifiable random functions with large input spaces. In Annual International Conference on the Theory and Applications of Cryptographic Techniques (pp. 656-672). Springer, Berlin, Heidelberg.

[PWHVNRG17] Papadopoulos, D., Wessels, D., Huque, S., Naor, M., Včelák, J., Reyzin, L., & Goldberg, S. (2017). Making NSEC5 practical for DNSSEC. Cryptology ePrint Archive.


[GLOW20] Galindo, D., Liu, J., Ordean, M., & Wong, J. M. (2021, September). Fully distributed verifiable random functions and their application to decentralised random beacons. In 2021 IEEE European Symposium on Security and Privacy (EuroS&P) (pp. 88-102). IEEE.

[irtf-vrf08] https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08

[SECG1]  Standards for Efficient Cryptography Group (SECG), "SEC 1: Elliptic Curve Cryptography", Version 2.0, May 2009, <http://www.secg.org/sec1-v2.pdf>.

