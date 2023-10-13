## Identifying Aborts

Identifying misbehaving participants efficiently is a key contribution of “CGMP21”. An abort will happen if any player deviates from the protocol in a clearly identifiable way by not complying with instructions. In the case of such an abort, the guilty party would have to be identified and removed. In this section, we analyse how the protocol can identify abortion and remove mmisbehaving participants.

### Abort Instances
Within the signing protocol, there are 3 instances that the protocol can abort. They can be summarized as follow.

1. In step 2 of the signing process, when an invalid proof \\(\pi_i\\) is detected.

2. In step 3 of the signing process, when an invalid proof \\(\pi_i^1\\) or \\(\pi_j^2\\) or \\(\pi_k^3\\) is detected for some \\(i,j,k\\).

3. In step 5 of the signing proces, when \\(g^{\delta}=\sum_i\Delta_i\\)

4. In step 6 of the signing process, when \\((r,s)\\) is not a valid signature of \\(M\\)

### How to Identify Abortions

Now, we show that how to identify the misbehaving participants that cause the protocol to abort in each cases above.

1. The first and second instances are straightforward. Whoever submits the wrong proof will be identified as malicious.

2. The third and fourth instance are much more complicated. At a high level, the identification of these two instances proceed as follow:

    1. Each participant \\(P_i\\) is asked to reprove that the ciphertexts \\(C_{ij}\\) are well formed for each \\(j \neq i\\). 
 
    2. Each participant \\(P_i\\) is asked to reveal \\(H_i=\mathsf{Enc_i}(k_i \gamma_i)\\) and \\(H_i'=\mathsf{Enc_i}(k_i w_i)\\) and prove their correctness given \\(K_i,G_i\\) and \\(X_i\\).

    3. Each participant \\(P_i\\) prove that \\(\delta_i\\) is the plaintext obtained via the ciphertext \\(U_i=H_i\prod_{j \neq i}C_{ij}F_{ji}=\mathsf{Enc_i}(k_i\gamma_i+\sum_{j \neq i}(\alpha_{ij}+\beta_{ij}))\\)
    
    4. Similarly, each participant \\(P_i\\) prove that \\(s_i\\) is the plaintext obtained via the ciphertext \\(V_i=K_i^m(H_i'\prod_{j \neq i}C_{ij}'F_{ji}')^r=\mathsf{Enc_i}(mk_i+r(k_iw_i+\sum_{j \neq i}(u_{ij}+v_{ij})))=\mathsf{Enc_i}(mk_i+r\sigma_i)\\)
    5. Whoever fails to prove will be identified as the malicious participant.