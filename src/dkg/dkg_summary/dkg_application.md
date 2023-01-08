### Applications

DKG has been used in numerous aspects: 

1. **Threshold Encryption and Signatures**: In threshold cryptosytems, one problem arises: single point of failure. If a single participant fails to follow the protocol, then the
entire system will abort. When applied as a component of a threshold cryptosystem, \\((n,t)\\)DKG solves this problem by ensuring that any \\(t+1\\) participants who behave honestly will allow the protocol to execute successfully.

1. **Identity based Cryptography (IBC)**: In IBC, a private-key generator (PKG) is required to generate a secret, called the master key and provide private keys to clients using it. Since he know the private key of the client, he can decrypt the messages of the client. A \\((n,t)\\) DKG solve this by distributing the PKG into many participants where any adversary who control at most \\(t\\) parties cannot compute the master key, and the client receive his secret key by collecting \\(t+1\\) partial private keys from the participants.

1. **Distriuted Pseudo-random Functions**: Pseudo-random Functions (PRF) and Verifiable Random Functions (VRF) is used to produce values that looks indistinguishable from random. Both require a secret key to compute the output. However, the secret key holder can select the value of the secret key to manipulate the output, or share the secret key with others to benefit himself. This can be prevented by applying a \\((n,t)\\) distributed version of PRF or VRF, thus no adversary can learn or affect the value of the secret key.

