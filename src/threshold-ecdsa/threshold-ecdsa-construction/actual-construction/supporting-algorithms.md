### Supporting Alorithms

In this section, we provide several supporting algorithms and protocols that support the signing protocol described above.

In Step 3 of the signing protocol, a participant who broadcasts \\(T=g^{\sigma}h^\ell\\) must prove the knowledge of \\(\sigma,\ell\\). The interactive protocol proceeds as follow:

1. The prover chooses \\(a,b \in \mathbb{Z}_p\\) and sends \\(\alpha=g^ah^b\\).
2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).
3. The prover sends \\(u=a+c\sigma\\) and \\(v=b+c\ell\\).
4. The verifier checks if \\(g^uh^v=\alphaT^c\\).