### Signing

In this section, we describe the signing process of the protocol.

- Each participant \\(P_i\\) choose \\(k_i,\gamma_i \in \mathbb{Z}_q\\) and compute \\(C_i=\mathsf{Com}(g^{\gamma_i})\\) and broadcasts \\(C_i\\).

- Each participant \\(P_i\\) broadcasts \\(\Gamma_i=g^{\gamma_i}\\), then compute \\(Gamma=\prod_i \Gamma_i\\) and 

$$R=\Gamma^{\delta^{-1}}=g^{\sum_i\gamma_ik^{-1}\gamma^{-1}}=g^{k^{-1}}$$

- Each participant \\(P_i\\) broadcasts \\(S_i=R^{\sigma_i}\\). If \\(pk \neq \prod_i S_i\\) then the protocol aborts.

- Each participants \\(P_i\\) broadcasts \\(\rho_i=m k_i+r \sigma_i\\). The final signature \\(\rho\\) of \\(m\\) is equal to be \\(\rho=\sum_{i} \rho_i\\).