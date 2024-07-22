## Possible IVC based construction

Apart from the previous well-known constructions, we now discuss another possible direction to construct a VDF is to use Incrementally Verifiable Computation (IVC) {{#cite BBBF18}}. The basic idea of IVC is that after each computation step, the prover can produce a proof of the current state. The proof can also be updated after each step to produce a new proof for the corresponding step. The definition and properties of IVC can be found in {{#cite Pa08}}.
Now, consider a IVC system **\\((\mathsf{IVC.Setup}, \mathsf{IVC.Prove}, \mathsf{IVC.Verify})\\)** for a function \\(f\\), it is possible to construct a VDF as follow:

**\\(\mathsf{Gen}(1^\lambda,t)\\)**: Run \\((ek,vk) \leftarrow\\) **\\(\mathsf{IVC.Gen}(1^\lambda,f,t)\\)**.

**\\(\mathsf{Eval}(X,ek,t)\\)**: Run and output \\((Y,\pi)\leftarrow\\) **\\(\mathsf{IVC.Prove}(X,ek,t)\\)**.

**\\(\mathsf{Verify}(X,Y,\pi,vk,t)\\)**: Outputs **\\(\mathsf{IVC.Verify}(X,Y,\pi,vk,t)\\)**.

The construction above is rather general. Therefore, it remains a question to choose a suitable IVC system and a function \\(f\\) to make the construction pratical.
As suggested by Boneh et al {{#cite BBBF18}} and Ethereum {{#cite KMT22}}, there are several candidate functions \\(f\\), for example VeeDo, Sloth++ {{#cite BBBF18}} or Minroot {{#cite KMT22}}.
Most previous IVCs involves using succinct non-interactive arguments of knowledge (SNARK). For these IVC constructions, at each step the prover has to constructs a SNARK which verifies a SNARK {{#cite BBBF18}}. This involves many FFT operations each step, and the SNARK verification circuit can be very expensive. In the recent NOVA {{#cite KST22}} proof system, the prover does not have to compute any FFT, and the recursion overhead is much lower than SNARK based IVCs {{#cite KST22}}. It is interesting to see if there will be any IVC based VDF using NOVA.
