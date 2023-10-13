### Signing

In this section, we describe the signing process of the protocol. For any set \\(S \in \\{1,\dots,n\\}\\) of \\(t+1\\) participants who participate to sign a message \\(M\\), let \\(w_i=\lambda_{i,S}\cdot sk_i \pmod{p}\\). Note that by Feldman's VSS, \\(sk=\sum_{i \in S} w_i\\). Note that since \\(pk_i=g^{sk_i} \\) is public after the key generation process, hence the value \\(W_i=g^{w_i}=pk_i^{\lambda_{i,\mathcal{S}}}\\) can also be publicly computed. The signing protocol follows a  \\(6\\) steps process below:

**Sign\\((M)\langle \\{P_i(sk_i)\\}_{i=1}^n\rangle\\):** 

1. Each participant \\(P_i\\) choose \\(d_{i},e_{i} \in \mathbb{Z_p}\\) and broadcasts \\((D_{i},E_{i})=(g^{d_{i},g^{e_{i}}})\\). Denote \\(B=\\{(i,D_i,E_i)\\}_{i \in S}\\).

   
2. For each \\(j \neq i\\), each \\(P_i\\) uses Schnorr protocol (see [Supporting Algorithms](./supporting-algorithms.md)) to check the validity of \\((D_i,E_i)\\). If any check fails then the protocol aborts.

3. Each \\(P_i\\) computes \\(\rho_j=\mathsf{H}(j,M,B)\\) for all \\(j \in S\\). Each \\(P_i\\) then computes the group commitment \\(R=\prod_{j \in S} D_jE_j^{\rho_j}\\) and the challenge \\(c=\mathsf{H}(R,\mathsf{pk},M)\\), then broadcasts \\((\rho_i,R,c)\\). 

4. Each \\(P_i\\) computes \\(z_i=d_i+e_i\rho_i+\lambda_{i,S}\cdot \mathsf{sk_i} \cdot c\\) and broadcasts \\(z_i\\).

5. Each \\(P_i\\) computes \\(R_i=D_iE_i^{\rho_i}\\) and broadcasts \\(R_i\\).

6. For each \\(i\\), participants check if \\(R=\prod_{i\in S}R_i\\) and \\(g_i=R_i\mathsf{pk_i}^{c \lambda_{i,S}}\\). If any check fails, report the misbehaving \\(P_i\\) and the protocol is aborted. Otherwise, compute \\(z=\sum_{i \in S}z_i\\) and returns \\(\sigma=(R,c,z)\\).

For abort identification, there are 3 instances that the protocol can abort in the signing protocol