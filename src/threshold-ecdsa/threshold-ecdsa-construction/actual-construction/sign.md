### Signing

In this section, we describe the signing process of the protocol. For any set \\(S \in \\{1,\dots,n\\}\\) of \\(t+1\\) participants who participate to sign a message \\(M\\), let \\(w_i=\lambda_{i,S}\cdot sk_i \pmod{p}\\). Note that by Feldman's VSS, \\(sk=\sum_{i \in S} w_i\\). Note that since \\(pk_i=g^{sk_i} \\) is public after the key generation process, hence the value \\(W_i=g^{w_i}=pk_i^{\lambda_{i,\mathcal{S}}}\\) can also be publicly computed. The signing protocol follows a  \\(6\\) steps process below:

**Sign\\((M)\langle \\{P_i(sk_i)\\}_{i=1}^n\rangle\\):** 

1. Each participant \\(P_i\\) choose \\(k_i,\gamma_i \in \mathbb{Z}_p\\) and do the following:

    1. Compute \\(K_i=\mathsf{Enc}_i(k_i), G_i=\mathsf{Enc}_i(\gamma_i)\\) 

    2. Compute a proof \\(\pi_i\\) certifying \\(k_i \in [1,2^{3\lambda}]\\) (see [Supporting Protocols](./supporting-algorithms.md)).

    3. Send \\((K_i,G_i,\pi_i)\\) to all participants. 
 
 Define \\(k=\sum_i k_i\\) and \\(\gamma=\sum_i \gamma_i\\). We see that 
\\(k\gamma=\sum _{i,j} k_i \gamma_j \pmod{p}\\) and \\(k\cdot sk=\sum _{i,j} k_i w_j \pmod{p}\\).

2. For each \\(j \neq i\\), each participant \\(P_i\\) do the following:

    1. Verify the validity of \\(\pi_j\\). If any check fails, the protocol aborts.

    2. Sample \\(\beta_{ij},v_{ij} \in [1,\dots,2^{7\lambda}]\\)

    3. Comute \\(C_{ji}=\mathsf{Enc_j}(\gamma_ik_j-\beta_{ij})=\gamma_i\cdot K_j-\mathsf{Enc_j}(\beta_{ij})\\) and \\(C_{ji}'=\mathsf{Enc_j}(w_ik_j-v_{ij})=w_i\cdot K_j-\mathsf{Enc_j}(v_{ij})\\)

    4. Compute \\(F_{ji}=\mathsf{Enc_i}(\beta_{ij})\\), \\(F_{ji}'=\mathsf{Enc_i}(v_{ij})\\), \\(\Gamma_i=g^{\gamma_i}\\) and a proof \\(\pi_i^1\\) which proves that \\(G_i=\mathsf{Enc_j}(\gamma_i)\\), \\(\Gamma_i=g^{\gamma_i}\\) and \\(\gamma_i<2^{3\lambda}\\).  The generation of \\(\pi_1^i\\) can be seen in [Supporting Protocols](./supporting-algorithms.md)

    5. Compute the proof \\(\pi_i^2\\) which prove that \\((C_{ji},W_i,K_j,F_{ji},\gamma_i,\beta_{ij})\\) satisfy the following relations
    
   
        - \\(C_{ji}=\gamma_i\cdot K_j-\mathsf{Enc_j}(\beta_{ij}) \\)
        - \\(\Gamma_i=g^{\gamma_i} \\)
        - \\(F_{ji}=\mathsf{Enc_2}(\beta_{ij}) \\)
        - \\(\beta_{ij} \le 2^{7\lambda} \\)
        - \\(\gamma_i \le 2^{3\lambda} \\)
 

    The generation of \\(\pi_i^2\\) can be seen in [Supporting Protocols](./supporting-algorithms.md).
     

    6. Compute the proof \\(\pi_i^3\\), which prove that \\((C_{ji}',\Gamma_i,K_j,F_{ji}',w_i,v_{ij})\\) satisfy the following relations 
        - \\(C_{ji}'=w_i\cdot K_j-\mathsf{Enc_j}(v_{ij}) \\)
        - \\(W_i=g^{w_i}\\)
        - \\(F_{ji}'=\mathsf{Enc_2}(v_{ij})\\)
        - \\(v_{ij}<2^{7\lambda} \\)
        - \\(w_i \le 2^{3\lambda}  \\)
    
    The generation of \\(\pi_i^3\\) can be seen in [Supporting Protocols](./supporting-algorithms.md).

    7. Send \\(C_{ji},C_{ji}',F_{ji},F_{ji}',\Gamma_i,\pi_i^1,\pi_i^2, \pi_i^3\\) to all participants.

3. For each \\(j \neq i\\), each participant \\(P_i\\) do the following:

    1. Verify the validity of \\(\pi_j^1,\pi_j^2,\pi_j^3\\). If any check fails, then the protocol aborts.

    2. Compute \\(\alpha_{ij}=\mathsf{Dec_i}(C_{ij})\\) and \\(u_{ij}=\mathsf{Dec_i}(C_{ij}') \\). Note that \\(\alpha_{ij}+\beta_{ij}=\gamma_i k_j\pmod{p}\\) and \\(u_{ij}+v_{ij}=w_i k_j \pmod{p}\\).
  
    3. Set \\(\delta_i=k_i\gamma_i+\sum_{j \neq i}(\alpha_{ij}+\beta_{ij}) \pmod{p}\\) and \\(\sigma_i=k_iw_i+\sum_{j \neq i}(u_{ij}+v_{ij})\pmod{p}\\). Note that \\(k\gamma=\sum_i\delta_i \pmod{p}\\) and \\(k\cdot sk=\sum_i \sigma_i \pmod{p}\\).


4. Each participant \\(P_i\\) computes \\(\Gamma=\prod_i \Gamma_i=g^\gamma\\), \\(\Delta_i=\Gamma^{k_i}=g^{\gamma k_i}\\) and send \\(\delta_i,\Delta_i\\) to all participants.

5. Each participant \\(P_i\\) sets \\(\delta=\sum_i\delta_i=k\gamma\\) and verify that \\(g^{\delta}=\sum_i\Delta_i\\). If any check fails, the protocol aborts. Otherwise, set \\(R=\Gamma^{\delta^{-1}}=g^{\gamma(k\gamma)^{-1}}=g^{k^{-1}}\\) and \\(r=R.\mathsf{x}\\).


6. Each participants \\(P_i\\) computes \\(m=\mathsf{H}(M)\\), then broadcasts \\(s_i=m k_i+r \sigma_i \pmod{p}\\). and set \\(s=\sum_{i} s_i=k(m+r\cdot sk) \pmod{p}\\). If **Verify\\(((M,(r,s),pk)=1\\)** then \\((r,s)\\) is a valid signature of \\(M\\), otherwise aborts.

