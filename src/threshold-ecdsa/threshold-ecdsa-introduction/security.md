### Security Properties

A threshold signature scheme should satisfy the following properties:

**Correctness:** For any set \\(\mathcal{S} \subset \\{1,\dots,n\\}\\) with \\(|\mathcal{S}|=t+1\\), if \\(P_i\\) are honest for all \\(i \in \mathcal{S}\\) and \\(s \leftarrow Sign(M,\mathcal{S})\langle \\{P_i(sk_i)\\}_{i \in \mathcal{S}}^t\rangle\\), then it holds that \\(Verify((M,s,pk)=1\\).

**Unforgability:** A \\((t-n)\\) threshold signature scheme is unforgeable if for any adversary \\(\mathcal{A}\\) who corrupts up to \\(t\\) participants and given previous signatures \\(s_1,\dots,s_k\\) of previous messages \\(M_1,\dots,M_k\\), cannot produce a valid signature \\(s\\) on an unsigned message \\(M\\) except with negligible probability.