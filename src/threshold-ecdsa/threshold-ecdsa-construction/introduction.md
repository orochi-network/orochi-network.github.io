## Gennaro's Construction

In this section we briefly describe the threshold ECDSA protocol of Gennaro etal in {{#cite CGGMP21}}, in which we assume that the readers have some familiarity to ECDSA signature scheme. Recall that the ordinary ECDSA signature scheme \\(\sigma=(r,s)\\) of a message \\(M\\) is generated as follow

$$r=R.\mathsf{x},~R=g^{k^{-1}},~m=\mathsf{H}(M)~\text{and}~s=k(m+r\cdot sk)$$

where \\(k \leftarrow \mathbb{Z}_p\\) and \\(sk\\) is the signer's secret key. Gennaro's protocol aims to provide a valid ECDSA signature of \\(M\\) above via a threshold manner. In addition, the protocol also provide the following features:

- **Universal Composable Security:** Gennaro etal's protocol achieve security in the Universal Composable Security framework. The framework is better in realizing the security of protocol, compared to traditional game based definitions.

- **Proactive Key Refresh:** Gennaro etal's protocol allow participants to refresh their partial secret keys after every epoch while not changing the grand secret key and public key. The goal of key refresh is to achieve security against proactive adversaries, who might corrupt participants for a certain period of time during the execution of the protocol. 

- **Non Interactive online phase:** Gennaro etal's protocol achieves non-interactive in the following sense: The signing protocol consists of a preprocessing phase before the  message \\(M\\) is known, followed by a non-interactive signing phase, where each participant can generate his own signature of \\(M\\) using the preprocessed information.

- **Identifiable Abort:** The protocol contains additional mechanisms that allow participants to detect any signers who fail to participate in the signing process, or deviate from it. Identifying misbehaving signers can be crucial for some applications. In most applications, being able to identify rogue servers is a convenience, allowing reboot the whole system.

To see how the protocol achieve the abovementioned properties, we now move to the actual construction of Gennaro and describe it.  

