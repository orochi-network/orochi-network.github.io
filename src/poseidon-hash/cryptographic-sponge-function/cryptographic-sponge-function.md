# Cryptographic sponge functions

In 2007, the sponge construction was introduced by Guido Bertoni and others.

A ***sponge construction*** or ***sponge function*** takes input bit data of arbitrary length (called a ***message***) and outputs bit data of *fixed* length (called a ***hash digest***). In the context of a sponge function, we can say that the message is ***absorbed*** to the sponge, and the digest is ***squeezed*** out.

A sponge construction hash \\(2\\) ***phases***:
+ ***Absorb***: the message is compressed iteratively.
+ ***Squeeze***: the digest is extracted iteratively.

Below is the construction of a sponge function:

![Sponge](https://keccak.team/images/Sponge-150.png)

1. The first component (the leftmost) is the ***state memory*** \\((S)\\) and it is divided into \\(2\\)parts: the ***Bitrate*** part of size \\(r\\)and the ***Capacity*** part of size \\(c\\). \\(S\\) is initially set to \\(0\\).
2. The second component is the ***compress function \\(f\\)*** whose input and output are of the same length. \\(S\\) is updated whenever passing the compress function.
3. The third component is the ***padding function \\(pad\\)*** which increases the size of the ***message \\(M\\)*** to the suitable length (specifically, the length of the padded message is a multiple of bitrate \\(r\\)).
4. The squeeze phase takes the Bitrate of \\(S\\) as output parts, before going to the compress function \\(f\\). The final output: \\(Z = \\{ Z_0; Z_1;...\\} \\). If \\(Z\\) length is not a multiple of \\(r\\), it will be truncated.

In Poseidon hash, we use the \\(Poseidon^\pi\\) permutation in place of the compression function.