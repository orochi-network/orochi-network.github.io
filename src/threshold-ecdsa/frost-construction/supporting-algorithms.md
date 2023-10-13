### Supporting Protocols

In this section, we specify the supporting protocols that support the signing protocol described in the previous section.

#### Feldman's VSS

Recall that in Step 2 of the key generation protocol, each participant \\(P\\) has to perform Feldman's VSS to share his secret \\(s\\) to other participants \\(P_i\\). The process of Feldman's VSS is described as follow:

1. \\(P\\) generate a random degree \\(t\\) polynomial \\(f(x)=a_0+a_1x+\dots+a_tx^t\\) such that \\(a_0=s\\), then broadcast \\(A_i=g^{a_i}\\) for \\( i \in \\{0,1,\dots,t\\}\\). Finally \\(P\\) secretly send the share \\(s_i=f(i)\\) to the \\(i\\)-th participant \\(P_i\\). 

2. Each participant \\(P_i\\) can verify the correctness of his share \\(s_i\\) by checking \\(g^{s_i}=\prod_{j=0}^tA_j^{i^j}\\). If the check fails, \\(P_i\\) broadcasts a complaint to \\(P\\). If \\(P\\) receives a complaint he will be disqualified. 


#### Zero Knowledge Proofs

**Schnorr Protocol:**

In Step 3 of the initial key generation process, a participant who broadcasts \\(pk_i=g^{sk_i}\\) must prove the knowledge of \\(sk_i\\) using Schnorr protocol. Schnorr protocol can be described as follow:

1. The prover chooses \\(a \in \mathbb{Z}_p\\) and sends \\(\alpha=g^a\\).

2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).

3. The prover sends \\(u=a+c\sigma\\).

4. The verifier checks if \\(g^u=\alpha\cdot pk_i^c\\).

