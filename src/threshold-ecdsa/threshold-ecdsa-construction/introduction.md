## Canneti's Construction

In this section we briefly describe the threshold ECDSA protocol of Canneti etal in {{#cite CGGMP21}}, in which we assume that the readers have some familiarity to ECDSA signature scheme. Recall that the ordinary ECDSA signature scheme \\(\sigma=(r,s)\\) of a message \\(M\\) is generated as follow

$$r=R.\mathsf{x},\ R=g^{k^{-1}},\ m=\mathsf{H}(M)\ \text{and}\ s=k(m+r\cdot sk)$$

where \\(k \leftarrow \mathbb{Z}_p\\) and \\(sk\\) is the signer's secret key. Canneti's protocol aims to provide a valid ECDSA signature of \\(M\\) above via a threshold manner. We now move to the actual construction of Canneti and describe it.  

