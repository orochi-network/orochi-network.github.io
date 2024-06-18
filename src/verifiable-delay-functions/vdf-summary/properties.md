### Properties

We require a VDF to have the following security properties:

**Correctness:** For all parameter \\(x\\),\\(t\\) and \\((ek,vk) \leftarrow \mathsf{Gen}(1^{\lambda})\\) if \\((Y,\pi)=Eval(ek,X,t)\\) then \\(Verify(vk,X,Y,\pi,t)=1\\)

**Soundness:** A VDF is sound if every algorithms \\(\mathcal{A}\\) can solve the following problem with negligible probability in \\(\lambda\\): Given \\(ev,vk\\) output \\(X,Y,\pi\\) such that \\((Y,\pi) \neq Eval(ek,X,t)\\) and \\(Verify(vk,X,Y,\pi,t)=1\\).

**Sequentiality:** A VDF is \\(t-\\)sequentiality if for all algorithms \\(mathcal{A}\\) with at most \\(\mathcal{O}(\mathsf{poly}(t))\\) parallel processors and runs within time \\(\mathcal{O}(\mathsf{poly}(t))\\), the experiment \\(ExpSeq_{VDF}^{\mathcal{A}}(1^\lambda)\\) is negilible in \\(\lambda\\), where \\(ExpSeq_{VDF}^{\mathcal{A}}(1^\lambda)\\) is described as follows:

\\(ExpSeq_{VDF}^{\mathcal{A}}(1^\lambda)\\):

- \\((ek,vk) \leftarrow \mathsf{Gen}(1^{\lambda})\\)
- \\(X {\stackrel{\$}{\leftarrow}} \mathcal{X}\\)
- \\ ((Y,\pi) \leftarrow \mathcal{A}(X,ek,vk)\\)
- Return \\((Y,\pi)==\mathsf{Eval}(X,ek,t)\\)