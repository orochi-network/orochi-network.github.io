### Security Properties

A threshold signature scheme should satisfy the following properties:

**Correctness:** For any set \\(\mathcal{S} \subset \\{1,\dots,n\\}\\) with \\(|\mathcal{S}|=t+1\\), if \\(P_i\\) follows the protocol for all \\(i \in \mathcal{S}\\) and 
\\(\sigma \leftarrow\\) **Sign\\((M,\mathcal{S})\langle \\{P_i(sk_i)\\}_{i \in \mathcal{S}}\rangle\\)**, then it holds that **Verify\\(((M,\sigma,pk)=1\\)**.

**Unforgability:** A \\((t-n)\\) threshold signature scheme is unforgeable if for any adversary \\(\mathcal{A}\\) who corrupts up to \\(t\\) participants and given previous signatures \\(\sigma_1,\dots,\sigma_k\\) of previous messages \\(M_1,\dots,M_k\\), cannot produce a valid signature \\(\sigma\\) on an unsigned message \\(M\\) except with negligible probability.