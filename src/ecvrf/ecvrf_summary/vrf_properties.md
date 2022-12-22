

### VRF Security Properties
We need a VRF to satisfy the following properties:

**Correctness:** If \\((Y,\pi)=Eval(sk,X)\\) then \\(Verify(pk,X,Y,\pi)=1\\)

**Uniqueness:** There do not exist tuples \\((Y,\pi)\\) and \\(Y',\pi'\\) with \\(Y \ne Y'\\) and:
\\(\mathsf{Verify}(pk,X,Y,\pi)=\mathsf{Verify}(pk,X,Y',\pi')=1\\)

**Pseudorandomess:** For any adversary \\(\mathcal{A}\\) the probability \\(|Pr[ExpRand^A_{VRF}(\lambda)=1]-\dfrac{1}{2}|\\) is negilible where \\(ExpRand_{VRF}^{\mathcal{A}}(1^\lambda)\\) is defined as follows:

\\(ExpRand_{VRF}^{\mathcal{A}}(1^\lambda)\\):

- \\((sk,pk) \leftarrow \mathsf{Gen}(1^{\lambda})\\)
- \\((X^*,st) \leftarrow \mathcal{A}^{\mathcal{O_{VRF}}(.)}(pk)\\)
- \\(Y_0 \leftarrow \mathsf{Eval}(X*,sk)\\)
- \\(Y_1 \leftarrow \\{0,1\\}^{out(\lambda)}\\)
- \\(\\{0,1\\} {\stackrel{\$}{\leftarrow}} b\\)
- \\(b' \leftarrow \mathcal{A}(Y_b,st)\\)
- Return \\(b=b'\\)

The oracle \\(\mathcal{O_{VRF}}(.)\\) works as follow: Given an input \\(x\\), it outputs the VRF value computed by \\(x\\) and its proof.

In the paper of \cite{PWHVNRG17}, the authors stated that a VRF must also be collision resistant. This property is formally defined below:

**Collision Resistant:** **Collision Resistant:** For any adversarial prover \\(\mathcal{A}=(\mathcal{A_1},\mathcal{A_2})\\) the probability \\(Pr\left[ExpCol_{VRF}^\mathcal{A}(1^\lambda)=1\right]\\) is negligible where \\(ExpCol_{VRF}^\mathcal{A}(1^\lambda)\\) is defined as follows: 

\\(ExpCol_{VRF}^\mathcal{A}(1^\lambda)\\):

- \\((pk,sk) \leftarrow \mathcal{A_1}(\mathsf{Gen}(1^{\lambda}))\\)
- \\((X,X',st) \leftarrow \mathcal{A_2}(sk,pk)\\)
- Return \\(X \ne X'\\) and \\(\mathsf{Eval}(X,sk)=\mathsf{Eval}(X',sk)\\)


It is interesting to see that, VRF can be used for signing messages. However, several signing algorithms such as ECDSA cannot be used to build a VRF. For a given message and a secret key, there can be multiple valid signatures, thus an adversiral prover could produce different valid outputs from a given input, and choose the one that benefits him. This contradict the uniqueness property of VRF. 


