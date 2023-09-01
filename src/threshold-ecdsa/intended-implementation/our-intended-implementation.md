## Our Intended Implementation
In this section, we are going to specify our intended implementation.  

First, we need to specify what curve are we going to use. Recall that the security lies in the discrete log assumption, hence it sufficies to choose any curve where the discrete log assumption is hard. The three most promising curves supported in our implementation are **secp256k1**, **edd25519** and **sr25519**.

The protocol also require a hash function in step. We choose the **keccak** function as our hash function.