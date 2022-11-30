




### Why using ECVRF

There are many VRF constructions, as listed in the history of VRF. However, ECVRF is chosen because its efficient in both proving and verification complexity, and can be make distributed. 

For example, in the construction of Hohenberger and Waters [HW10], the proof of the VRF contains \\(\Theta(\lambda)\\) group elements. The number of evaluation steps is \\(\Theta(\lambda)\\), because we need to compute \\(\Theta(\lambda)\\) group elements. Verification require \\(\Theta(\lambda)\\) computations using bilinear map, where \\(\lambda\\) is the security parameter. It is unknown whether this VRF can be made distributed or not.

In the other hand, the proof size and the number of evaluation and verification steps of ECVRF are all constant and can be make distributed, as in the paper of Galindo et al [GLOW20]. The VRF construction of [DY05] also have constant proof size, evaluation and verification steps, and can be make distributed. However, in their paper, they require a cyclic group whose order is a 1000 bit prime, while ECVRF require a cyclic group whose order is a 256 bit prime such that the Decisional Diffie-Hellman (DDH) assumption holds. Hence, we can implement the VRF using Secp256k1, a curve used by Ethereum for creating digital signature. The embeeding degree of Secp256k1 is large, thus the DDH assumption is believed to be true.

