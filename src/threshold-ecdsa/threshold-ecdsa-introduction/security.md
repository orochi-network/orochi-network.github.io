### Security Properties

A threshold signature scheme should satisfy the following properties:

**Correctness:** If all participants are honest, then at the end of the signing protocol, the verify algorithm always returns \\(1\\).

**CMA Unforgability:** For any adversary who corrupts up to \\(t\\) participants,