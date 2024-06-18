### VDF Algorithms

A VDF consists of three algorithms \\( (\mathsf{Gen}, \mathsf{Eval}, \mathsf{Verify})\\) where:

**\\((ek,vk) \leftarrow \mathsf{Gen}(1^{\lambda},t)\\):** This algorithm takes as input as a security parameter \\( \lambda \\), and outputs a public key pair \\( (ek,vk)\\).  
**\\( (Y,\pi) \leftarrow \mathsf{Eval}(X,ek,t)\\):** This algorithm takes an input an evaluation key \\(sk\\), a value \\(X\\) a time parameter \\(t\\) and outputs a value \\(Y \in {0,1}^{out(\lambda)} \\) and a proof \\( \pi \\).
**\\( b \leftarrow \mathsf{Verify}(vk,X,Y,\pi,t)\\):** This algorithm takes an input a verification key \\(vk \\), a value \\(X\\), a value \\(Y\\), a proof \\(\pi\\), a time parameter \\(t\\) and outputs a bit \\(b\\) that determines whether \\(Y=\mathsf{Eval}(X,ek)\\). This algorithm run within time \\(\mathcal{O}(poly(log(t)))\\).
