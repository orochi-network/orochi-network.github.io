### Signing

We describe the sign procedure of threshold ECDSA construction in. The procedure is divided into four smaller round below

**Pre-sign:**

We recall that after the **KeyGen** protocol, the secret state each participants \\(P_i\\) consists of \\(sk_i,p_i,q_i\\) such that \\(N_i=p_i \cdot q_i\\). 

- **Round 1:**
 - Sample \\(k_i\\) and \\(\gamma_i\\) uniformly in \\(\mathbb{Z}_q\\).

- **Round 2:**

- **Round 3:**

 
 - Set \\(\delta=\sum_i\delta_i\\), then check if \\(g^\delta=\prod_i\Delta_i\\)and set \\(\rho=\Gamma^{\delta^{-1}}\\).

**Sign(\\(msg,\rho,\\{\sigma_i\\}_{i=1}^{t+1}\\)):** 

This algorithm is used by each participant to produce the final signature. Each \\(P_i\\) do the following:

- Set \\(r\\) to be the \\(x\\)-coordinate of \\(R\\) and compute \\(\sigma_i=k_iM+r\chi_i\\)

- Set \\(\sigma=\sum_{i}\sigma_i\\) check if 
(\\(\rho,\sigma\\)) is a valid signature using **Verify** alorithm.

- Output \\((\rho,\sigma)\\).

Note that the signing algorithm above can fail if **Verify** returns False. In this case, we need to track down the curprit who failed to follow the protocol as follow: