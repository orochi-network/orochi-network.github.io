## FROST's Construction

In this section we briefly describe the FROST threshold Schnorr protocol in {{#cite CGGMP21}}, in which we assume that the readers have some familiarity to Schnorr signature scheme. Recall that the ordinary Schnorr signature scheme (as well as its variant such as EDDSA), the signature \\(\sigma=(R,c,z)\\) of a message \\(M\\) is generated as follow

$$R=g^r,\ c=\mathsf{H}(R||\mathsf{pk}||M)\ \text{and}\ z=c+r\cdot sk$$

where \\(r \leftarrow \mathbb{Z}_p\\) and \\(sk\\) is the signer's secret key. FROST protocol aims to provide a valid Schnorr signature of \\(M\\) above via a threshold manner. We now move to the actual construction of FROST and describe it.  

