### Signing

In this section, we describe the signing process of the protocol. It follows a  \\(7\\) steps process below:

**Sign\\((M)\langle \\{P_i(sk_i)\\}_{i=1}^n\rangle\\):** 

1. Each participant \\(P_i\\) choose \\(k_i,\gamma_i \in \mathbb{Z}_q\\) and compute \\(C_i=\mathsf{Com}(g^{\gamma_i})\\) and broadcasts \\(C_i\\).

Define \\(k=\sum_i k_i\\) and \\(\gamma=\sum_i \gamma_i\\). We see that 
\\(k\gamma=\sum _{i,j} k_i \gamma_j\\).

2. Each pair of participant \\(P_i,P_j\\) engages in

3. Each participant \\(P_i\\) broadcasts the following:

    - \\(\delta_i\\) and reconstructs \\(\delta=\sum_i\delta_i=k\gamma\\).
    - \\(T_i=g^{\sigma_i}h^{\ell_i}\\) and proves that he knows \\(\sigma_i,\ell_i\\).

4. Each participant \\(P_i\\) broadcasts \\(\Gamma_i=g^{\gamma_i}\\), then compute \\(Gamma=\prod_i \Gamma_i\\) and 

$$R=\Gamma^{\delta^{-1}}=g^{\sum_i\gamma_ik^{-1}\gamma^{-1}}=g^{k^{-1}}$$

5. Each participant \\(P_i\\) broadcasts \\(V_i=R^{k_i}\\). If \\(g \neq \prod_{i} V_i\\) then the protocol aborts. 

6. Each participant \\(P_i\\) broadcasts \\(S_i=R^{\sigma_i}\\) and a zero  knowledge proof of consistency between \\(S_i\\) and \\(T_i\\). If \\(pk \neq \prod_i S_i\\) then the protocol aborts.

7. Each participants \\(P_i\\) computes \\(m=\mathsf{H}(M)\\) and broadcasts \\(s_i=m k_i+r \sigma_i\\). FInally, the signature \\(s\\) of \\(msg\\) is equal to be \\(s=\sum_{i} s_i\\).