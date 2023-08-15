## Gennaro's Construction

In this section we briefly describe the threshold ECDSA protocol of Gennaro etal in. This is the protocol that provide the best security and efficiency so far. As the title suggest, the protocol provide the following features:

- **Non Interactive:** Gennaro etal's protocol achiveves non-interactive signing in the sense that: 
- **Universal Composable Security:** The security of the protocol can be proven with the Universal Composable framework. The framework can realize protocol execution in a realistic enviromnent, compared to the traditional game based security.
- **Proactive Security:** 
- **Identifiable Abort:** The protocol contains additional mechanisms that allow participants to detect any signers who fail to participate in the signing process, or deviate from it.

To see how the protocol achieve the abovementioned properties, we will move to construction of the protocol and analyse it.