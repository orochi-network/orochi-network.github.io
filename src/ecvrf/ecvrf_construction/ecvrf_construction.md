

### Public Parameters

Let \\(\mathbb{G}\\) be a cyclic group of prime order \\(p\\) with generator \\(g\\). Denote \\(\mathbb{Z}_p\\) to be the set of integers modulo \\(p\\). Let \\(\mathsf{HashToCurve}\\) be a hash function mapping a bit string to an element in \\(\mathbb{G}\\). Let \\(\mathsf{HashPoints}\\) be a hash function mapping arbitary input length to a \\(256\\) bit integer.

Note that, in the paper of {{#cite PWHVNRG17}}, the functions \\(\mathsf{HashToCurve}\\) and \\(\mathsf{HashPoint}\\) are modeled as random oracle model. This is used to prove the security of the VRF. 

The cofactor parameter mentioned in the irtf draft is set to \\(1\\).


### ECVRF Construction



**\\(\mathsf{KeyGen}(1^{k})\\):** Choose a random secret value \\(sk \in 
\mathbb{Z}_p\\) and the secret key is set to be \\(sk \\). The public key
is  \\(pk=g^{sk}\\).                                                     


**\\(\mathsf{Eval}(sk,X)\\):** Given the secret key \\(sk\\) and an input \\(X\\), the pseudorandom value \\(Y\\) and its proof \\(\pi\\) is computed as follow:

1. Compute \\(h=\mathsf{HashToCurve}(X,pk)\\).

1. Compute \\(\gamma=h^{sk}\\).

1. Choose a value \\(k\\) uniformly in \\(\mathbb{Z}_p\\).

1. Compute \\(c=\mathsf{HashPoint}(g,h,pk,gamma,g^k,h^k)\\)

1. Compute \\(s \equiv k-c.sk \pmod{q}\\)

1. Compute \\(gammastr=\mathsf{PointToString}(\gamma)\\)

1. The output Y of the VRF is computed as \\(Y=\mathsf{SHA256}(gammastr)\\)

1. The proof \\(\pi\\) of the VRF is computed as \\(\pi=(\gamma,c,s)\\) 

**\\(\mathsf{Verify}(pk,X,Y,\pi)\\):** Given the public key \\(pk\\), the VRF input \\(X\\), the VRF output \\(Y\\) and its proof \\(\pi=(\gamma,c,s)\\), the verification step proceed as follow:

1. Compute \\(u=pk^cg^s\\)

1. Compute \\(h=\mathsf{HashToCurve}(X)\\)

1. Compute \\(v=\gamma^ch^s\\)

1. Check if \\(c=\mathsf{HashPoint}(g,h,pk,gamma,g^k,h^k)\\) and \\(Y=\mathsf{ProofToHash}(\gamma)\\)
