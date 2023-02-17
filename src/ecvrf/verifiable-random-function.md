# Verifiable Random Function (VRF)

We present an overview of verifiable random functions (VRF) and describe a construction a VRF based on elliptic curves in {{#cite PWHVNRG17}}.

Informally speaking, a VRF is a function that generates values that looks indistinguishable from random, and these values can be verified if they were computed correctly. Later, we discuss a few cryptographic applications that VRF possibly plays an important building blocks in constructing them.

The chapter is separated into \\(2\\) two major parts. In the first part, we state the formal definition of VRF including its syntax and security properties. Then we talk about the history of VRFs to see its development. In the second major part, we describe the VRF based on elliptic curve and our implementation of the VRF. 
