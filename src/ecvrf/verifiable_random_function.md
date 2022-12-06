# Verifiable Random Function (VRF)
We present an overview of VRF and describe a construction a VRF based on elliptic curves in [PWHVNRG17].

Informally speaking, a Verifiable random function (VRF) is a function that generates values that looks indistinguishable from random, and these values can be verified if they were computed correctly. Later, will explain why VRF are important and what are they used for. 

The blog is seperated into \\(2\\) two major parts. In the first major part, we state the formal definition of VRF and its security properties. Then we talk about the history of VRF and their application in blockchain. In the second major part, we describe the VRF based on elliptic curve and our implementation of the VRF. 