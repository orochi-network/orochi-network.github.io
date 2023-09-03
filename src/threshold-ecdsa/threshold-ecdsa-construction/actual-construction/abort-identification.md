## Identifying Aborts

Identifying misbehaving participants efficiently is a key contribution of “GG20”. An abort will happen if any player deviates from the protocol in a clearly identifiable way by not complying with instructions. In the case of such an abort, the guilty party would have to be identified and removed. In this section, we analyse how the protocol can identify abortion and remove mmisbehaving participants.

### Abort Instances
Within “GG20”’s Sign protocol, there are eight instances in which an abort can occur, according to {{#cite GG20}}. They can be summarized as follow.

1. In Step 2,3,5,6, the protocol aborts a participant \\(P_i\\) failed to produce a valid zero knowledge proof or valid opening of commitments.

2. In step 7 the protocol aborts when \\(r,s\\) is not a valid signature of \\(M\\)

3. In step 5, the protocol aborts when \\(g \neq \prod_{i} V_i\\).

4. In step 6, the protocol aborts when \\(pk \neq \prod_i S_i\\).

### How to Identify Abortations

Now, we show that how to identify the misbehaving participants that cause the protocol to abort in each cases above.

1. The first case happens when a player failing a zero knowledge proof (Step 2,3,5,6) or a commitment opening (Step 4) and are easily concluded. 

2. In the second case, when \\((r, s)\\) is not a valid signature for \\(M\\), identification is slightly more complicated. In this case, Each \\(P_i\\) would thus be asked to confirm \\(R^{S_i}=V^mS_i^r\\). Whoever fails is identified as the malicious participant.

3. In the third case, when \\(g \neq \prod_{i} V_i\\) and \\(pk \neq \prod_i S_i\\), participants will be forced to open their private input and output of the \\(\mathsf{MtA}\\) protocol (the values \\(k_i,\gamma_i,\alpha_{ij},\beta_{ij}\\)), which need not to be kept secret after the signature has been created. Other participants can compute \\(\delta_i\\), then check if \\(G^{\delta_i}=V_i\\) and the malicious participant will be identified.

4. Finally, when \\(pk \neq \prod_i S_i\\) participants will be forced to open \\(u_{ij}\\) and \\(k_i\\). Other participants can verify the correctness of \\(u_{ij}\\) then compute \\(g^{\sigma_i}\\) from \\(g^{w_i},k_i\\) and \\(u_{ij}\\). Given \\(S_i=R^{\sigma_i}\\), each participant \\(P_i\\) is required to prove the consistency of \\(sigma_i\\) from \\(S_i\\) and \\(g^{\sigma_i}\\). Anyone fails to do this will be identified as the malicious participant.