### Signing

In this section, we describe the signing process of the protocol. For any set \\(S \in \\{1,\dots,n\\}\\) of \\(t+1\\) participants who participate to sign a message \\(M\\), we denote \\(\lambda_{i,S}\\) to be the Lagrange coefficient w.r.t \\(S\\) and \\(w_i=\lambda_{i,S}sk_i\\). Note that by Feldman's VSS, \\(sk=\sum_{i \in S} w_i\\). The signing protocol follows a  \\(7\\) steps process below:

**Sign\\((M)\langle \\{P_i(sk_i)\\}_{i=1}^n\rangle\\):** 

1. Each participant \\(P_i\\) choose \\(k_i,\gamma_i \in \mathbb{Z}_q\\) and compute \\(C_i=\mathsf{Com}(g^{\gamma_i})\\) and broadcasts \\(C_i\\).

Define \\(k=\sum_i k_i\\) and \\(\gamma=\sum_i \gamma_i\\). We see that 
\\(k\gamma=\sum _{i,j} k_i \gamma_j\\) and \\(k\cdot sk=\sum _{i,j} k_i w_j\\).

2. Each pair of participant \\(P_i,P_j\\) engages in two sub-protocols that converts multiplicative shares into additive shares as follow:

    - \\(P_i,P_j\\) run \\(\mathsf{MtA}\\) with shares \\(k_i,\gamma_j\\) (see [Supporting Protocols](./supporting-algorithms.md)) to obtain secret outputs \\(\alpha_{ij},\beta_{ij}\\) for \\(P_i,P_j\\) respectively satisfying \\(k_i\gamma_j=\alpha_{ij}+\beta_{ij}\\)

    - \\(P_i,P_j\\) run \\(\mathsf{MtA}\\) with shares \\(k_i,w_j\\)  to obtain secret outputs \\(u_{ij},v_{ij}\\) for \\(P_i,P_j\\) respectively satisfying \\(k_i\gamma_j=u_{ij}+v_{ij}\\)

    - Each \\(P_i\\) sets \\(\delta_i=k_i\gamma_i+\sum_{j \neq i}(\alpha_{ij}+\beta_{ji})\\) and \\(\sigma_i=k_iw_i+\sum_{j \neq i}(u_{ij}+v_{ji})\\). Note that \\(k\gamma=\sum_i\delta_i\\) and \\(k\cdot sk=\sum_i \sigma_i\\).

3. Each participant \\(P_i\\) broadcasts the following:

    - \\(\delta_i\\) and reconstructs \\(\delta=\sum_i\delta_i=k\gamma\\).
    - \\(T_i=g^{\sigma_i}h^{\ell_i}\\) and proves that he knows \\(\sigma_i,\ell_i\\) (see [Supporting Protocols](./supporting-algorithms.md)).

4. Each participant \\(P_i\\) broadcasts \\(\Gamma_i=g^{\gamma_i}\\), then compute \\(\Gamma=\prod_i \Gamma_i\\) and 

$$R=\Gamma^{\delta^{-1}}=g^{\sum_i\gamma_ik^{-1}\gamma^{-1}}=g^{k^{-1}}$$ as well as \\(r=R.\mathsf{x}\\)

5. Each participant \\(P_i\\) broadcasts \\(V_i=R^{k_i}=g^{k^{-1}k_i}\\). If \\(g \neq \prod_{i} V_i\\) then the protocol aborts. 

6. Each participant \\(P_i\\) broadcasts \\(S_i=R^{\sigma_i}=g^{k^{-1}\sigma_i}\\) and a zero  knowledge proof of consistency between \\(S_i\\) and \\(T_i\\) (see [Supporting Protocols](./supporting-algorithms.md)). If \\(pk \neq \prod_i S_i\\) then the protocol aborts.

7. Each participants \\(P_i\\) computes \\(m=\mathsf{H}(M)\\) and broadcasts \\(s_i=m k_i+r \sigma_i\\). Finally, the signature \\(s\\) of \\(M\\) is equal to be \\(s=\sum_{i} s_i=k(m+r\cdot sk)\\).