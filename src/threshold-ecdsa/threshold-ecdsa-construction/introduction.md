## Gennaro's Construction

In this section we briefly describe the threshold ECDSA protocol of Gennaro etal in {{#cite GG20}}. As the title suggest, the protocol provide the following features:

- **Non Interactive online phase:** Gennaro etal's protocol achieves non-interactive in the following sense: The signing protocol consists of a preprocessing phase before the  message \\(M\\) is known, followed by a non-interactive signing phase, where each participant can generate his own signature of \\(M\\) using the preprocessed information.
- **Identifiable Abort:** The protocol contains additional mechanisms that allow participants to detect any signers who fail to participate in the signing process, or deviate from it. Identifying misbehaving signers can be crucial for some applications. In most applications, being able to identify rogue servers is a convenience, allowing masters to avoid restarting all servers.

To see how the protocol achieve the abovementioned properties, we now move to construction of the protocol and analyse it.