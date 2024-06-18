### Properties

We require a VDF to have the following security properties:

**Correctness:** For all parameter \\(x\\),\\(t\\) and \\((ek,vk) \leftarrow \mathsf{Gen}(1^{\lambda})\\) if \\((Y,\pi)=Eval(ek,X,t)\\) then \\(Verify(vk,X,Y,\pi,t)=1\\)

**Soundness:** A VDF is sound if every algorithms \\(A\\) can solve the following problem with negligible probability in \\(\lambda\\): Given \\(ev,vk\\) output \\(X,Y,\pi\\) such that \\((Y,\pi) \neq Eval(ek,X,t)\\) and \\(Verify(vk,X,Y,\pi,t)=1\\).

**Sequentiality:** A VDF is \\(t-\\)sequentiality if for all algorithms \\(A\\) with at most \\(O(\mathsf{poly}(t))\\) parallel processors and runs within time \\(O(\mathsf{poly}(t))\\), the experiment \\(ExpSeq*{VDF}^{A}(1^\lambda)\\) is negilible in \\(\lambda\\), where \\(ExpSeq*{VDF}^{A}(1^\lambda)\\) is described as follows:

\\(ExpSeq\_{VDF}^{A}(1^\lambda)\\):

- \\((ek,vk) \leftarrow \mathsf{Gen}(1^{\lambda})\\)
- \\(X {\stackrel{\$}{\leftarrow}} \\{0,1\\}^{in(\lambda)}\\)
- \\((Y,\pi) \leftarrow A(X,ek,vk)\\)
- Return \\((Y,\pi)==\mathsf{Eval}(X,ek,t)\\)
