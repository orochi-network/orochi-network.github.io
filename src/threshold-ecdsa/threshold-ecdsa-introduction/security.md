### Security Properties

A threshold signature scheme should satisfy the following properties:

**Correctness:** If all participants are honest, then at the end of the signing protocol, the verify algorithm always returns \\(1\\).

**Unforgability:** A \\(t-n\\) threshold signature is unforgeable if for any adversary who corrupts up to \\(t\\) participants and previous signatures \\(s_1,\dots,s_k\\) of previous messages \\(M_1,\dots,M_k\\), cannot produce a valid signature \\(s\\) on an unsigned message \\(M\\).