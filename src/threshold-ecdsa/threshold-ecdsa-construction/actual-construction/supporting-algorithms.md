### Supporting Protocols

In this section, we specify the supporting protocols that support the signing protocol described in the previous section.

#### Multiplicative to additive share conversion protocols 

In Step 2 of the signing protocol, participants \\(P_1\\) and \\(P_2\\) who holds secrets \\(a,b\\) respectively, must follow the \\(\mathsf{MtA}\\) protocol to receive secret outputs \\(\alpha,\beta\\) for \\(P_1\\) and \\(P_2\\) such that \\(ab=\alpha+\beta \pmod{p}\\). The protocol proceeds as follow:

1. \\(P_1\\) computes \\(c_A=\mathsf{Enc}(a)\\) and a proof \\(\pi_A\\) certifying the correctness of \\(c_A\\) and \\(a\le p^3\\). \\(P_1\\) then sends \\((c_A,\pi_A)\\) to \\(P_2\\).
2. \\(P_2\\) verifies \\(\pi_A\\) and aborts if it fails to verify. Otherwise, it does the following:
    - Choose \\(\beta \in \mathbb{Z}_{q^5}\\) and compute \\(\beta_2=-\beta \pmod{p}\\).
    - Compute \\(c_B=b*c_A+\mathsf{Enc}(\beta_2)=\mathsf{Enc}(ab+\beta')\\) and a proof \\(\pi_B\\) certifying the correctness of \\(c_B\\) and and \\(b \le p^3\\). Finally, send \\((c_B,\pi_B)\\) to \\(P_1\\).
3. \\(P_1\\) verifies \\(\pi_B\\) and aborts if it fails to verify.  Otherwise, it computes \\(\alpha=\mathsf{Dec}(c_B)=ab+\beta_2=ab-\beta \pmod{p}\\)  

#### Zero Knowledge Proofs
In Step 3 of the signing protocol, a participant who broadcasts \\(T=g^{\sigma}h^\ell\\) must prove the knowledge of \\(\sigma,\ell\\). The interactive protocol proceeds as follow:

1. The prover chooses \\(a,b \in \mathbb{Z}_p\\) and sends \\(\alpha=g^ah^b\\).
2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).
3. The prover sends \\(u=a+c\sigma\\) and \\(v=b+c\ell\\).
4. The verifier checks if \\(g^uh^v=\alpha T^c\\).


In Step 6 of the signing protocol, a participant who broadcasts \\(S=R^{\sigma}, T=g^\sigma h^\ell\\) must prove the knowledge of \\(\sigma,\ell\\). The interactive protocol proceeds as follow:

1. The prover chooses \\(a,b \in \mathbb{Z}_p\\) and sends \\(\alpha=g^ah^b\\) and \\(\beta=R^a\\).
2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).
3. The prover sends \\(u=a+c\sigma\\) and \\(v=b+c\ell\\).
4. The verifier checks if \\(g^uh^v=\alpha T^c\\) and \\(R^u=\beta S^c\\).