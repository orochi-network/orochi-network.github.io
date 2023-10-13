## FROST's Construction

In this section we briefly describe the FROST threshold Schnorr protocol in {{#cite KG20}}, in which we assume that the readers have some familiarity to Schnorr signature scheme. Recall that the ordinary Schnorr signature scheme, the signature \\(\sigma=(R,c,z)\\) of a message \\(M\\) is generated as follow

$$R=g^r,\ c=\mathsf{H}(R||\mathsf{pk}||M)\ \text{and}\ z=c+r\cdot sk$$

where \\(r \leftarrow \mathbb{Z}_p\\) and \\(sk\\) is the signer's secret key. The EDDSA signature scheme also produces the signature with the exact form above, with the only difference is that the nonce \\(r\\) is produced deterministically.  FROST protocol aims to provide a valid Schnorr signature of \\(M\\) above via a threshold manner. We now move to the actual construction of FROST and describe it.  

