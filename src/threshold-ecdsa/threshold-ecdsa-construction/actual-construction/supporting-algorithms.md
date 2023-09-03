### Supporting Protocols

In this section, we specify the supporting protocols that support the signing protocol described in the previous section.

#### Feldman's VSS

Recall that in Step 2 of the key generation protocol, each participant \\(P\\) has to perform Feldman's VSS to share his secret \\(s\\) to other participants \\(P_i\\). The process of Feldman's VSS is described as follow:

1. \\(P\\) generate a random degree \\(t\\) polynomial \\(f(x)=a_0+a_1x+\dots+a_tx^t\\), then broadcast \\(A_i=g^{a_i}\\) for \\( i \in \\{0,1,\dots,t\\}\\). Finally \\(P\\) secretly send the share \\(s_i=f(i)\\) to the \\(i\\)-th participant \\(P_i\\).

2. Each participant \\(P_i\\) can verify the correctness of his share \\(s_i\\) by checking \\(g^{s_i}=\prod_{j=0}^tA_j^{i^j}\\). If the check fails, \\(P_i\\) broadcasts a complaint to \\(P\\). If \\(P\\) receives a complaint he will be disqualified. 

#### Multiplicative to additive share conversion protocols 

In Step 2 of the signing protocol, participants \\(P_1\\) and \\(P_2\\) who holds secrets \\(a,b\\) respectively, must follow the \\(\mathsf{MtA}\\) protocol to receive secret outputs \\(\alpha,\beta\\) for \\(P_1\\) and \\(P_2\\) such that \\(ab=\alpha+\beta \pmod{p}\\). 

Assume that \\(P_1\\) is equipped with the Pallier encryption scheme \\(\mathcal{E}=(\mathsf{Enc},\mathsf{Dec})\\), described in {{#cite P99}}. The protocol proceeds as follow:

1. \\(P_1\\) computes \\(c_A=\mathsf{Enc}(a)\\) and a proof \\(\pi_A\\) certifying the correctness of \\(c_A\\) and \\(a\le p^3\\) (the proof \\(\pi_A\\) can be generated using the public parameters \\(N,h_1,h_2\\) in {{#cite MR04}}). \\(P_1\\) then sends \\((c_A,\pi_A)\\) to \\(P_2\\).
2. \\(P_2\\) verifies \\(\pi_A\\) and aborts if it fails to verify. Otherwise, it does the following:
    - Choose \\(\beta \in \mathbb{Z}_{q^5}\\) and compute \\(\beta_2=-\beta \pmod{p}\\).
    - Compute \\(c_B=b*c_A+\mathsf{Enc}(\beta_2)=\mathsf{Enc}(ab+\beta')\\) and a proof \\(\pi_B\\) certifying the correctness of \\(c_B\\) and \\(b \le p^3\\) and \\(\beta_2 \le q^7\\). Finally, send \\((c_B,\pi_B)\\) to \\(P_1\\).
3. \\(P_1\\) verifies \\(\pi_B\\) and aborts if it fails to verify.  Otherwise, it computes \\(\alpha=\mathsf{Dec}(c_B)=ab+\beta_2=ab-\beta \pmod{p}\\)  

#### Zero Knowledge Proofs

In Step 3 of the key generation protocol, a participant who broadcasts \\(pk_i=g^{sk_i}\\) must prove the knowledge of \\(sk_i\\) using Schnorr protocol. Schnorr protocol can be described as follow:

1. The prover chooses \\(a \in \mathbb{Z}_p\\) and sends \\(\alpha=g^a\\).
2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).
3. The prover sends \\(u=a+c\sigma\\).
4. The verifier checks if \\(g^u=\alpha pk_i^c\\).

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

#### Commitment Scheme

In Step 1 of the Key Generation and Signing protocol, we require participants to commit their messages using a commitment scheme \\(\mathsf{Com}\\). In practice, one can use a cryptographic hash function \\(\mathsf{H}\\) and define the commitment of \\(X\\) to be \\(\mathsf{H}(X,r)\\) where \\(r\\) is chosen uniformly. 