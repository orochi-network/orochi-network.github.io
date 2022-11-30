
### VRF Algorithms
A Verifiable random function consists of three algorithms \\( (\mathsf{Gen}, \mathsf{Eval}, \mathsf{Verify})\\) where:

**\\((pk,sk) \leftarrow \mathsf{Gen}(1^{\lambda})\\):** This algorithm takes as input as a security parameter \\( \lambda \\) and
outputs a key pair \\( (pk,sk)\\).

**\\( (Y,\pi) \leftarrow \mathsf{Eval}(X,sk)\\):** This algorithm takes as input a secret key \\(sk\\) and a value \\(X\\)
and outputs a value \\(Y \in {0,1}^{out(\lambda)} \\) and a proof \\( \pi \\).

**\\( b \leftarrow \mathsf{Verify}(pk,X,Y,\pi)\\):** This algorithm takes an input a public key \\(pk \\), a value \\(X\\), a value \\(Y\\), a proof \\(\pi\\) and outputs a bit \\(b\\) that determines whether \\(Y=\mathsf{Eval}(X,sk)\\).

