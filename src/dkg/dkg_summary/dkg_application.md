### Applications

DKG has been used in numerous aspects: 

1. Symmetric-key cryptography: 

1. Threshold Encryption and Signatures: In threshold cryptosytems, one problem arises: single point of failure. If a single participant fails to follow the protocol, then the
entire system will abort. When applied as a component of a threshold cryptosystem, \\((n,t)\\)DKG solves this problem by ensuring that any \\(t+1\\) participants who behave honestly will allow the protocol to execute successfully.

1. Identity based Cryptography (IBC): In IBC, a private-key generator (PKG) is required to generate a secret, called the master key and provide private keys to clients using it. Since he know the private key of the client, he can decrypt the messages of the client. A \\((n,t)\\) DKG solve this by distributing the PKG into many participants where any adversary who control at most \\(t\\) parties cannot compute the master key, and the client receive his secret key by collecting \\(t+1\\) partial private keys from the participants.

