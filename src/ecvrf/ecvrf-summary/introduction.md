### Introduction
In cryptography, a verifiable random function (VRF) is a public key version of a pseudorandom function. It produces a pseudorandom output and a proof certifying that the output is computed correctly. 

A VRF includes a pair of keys, named public and secret keys. The secret key, along with the input is used by the holder to compute the value of a VRF and its proof, while the public key is used by anyone to verify the correctness of the computation.

The issue with traditional pseudorandom functions is that their output cannot be verified without the knowledge of the seed. Thus a malicious adversary can choose an output that benefits him and claim that it is the output of the function. VRF solves this by introducing a public key and a proof that can be verified publicly while the owner can keep secret key to produce numbers indistinguishable from randomly chosen ones.

VRF has applications in various aspects. Among them, in internet security, it is used to provide privacy against offline enumeration (e.g. dictionary attacks) on data stored in a hash-based data structure [irtf-vrf15](https://datatracker.ietf.org/doc/draft-irtf-cfrg-vrf/). VRF is also used in lottery systems {{#cite MR02}} and E-cashes {{#cite BCKL09}}.

