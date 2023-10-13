### Verification

Recall that the verification algorithm in threshold Schnorr remain identical to an ordinary Schnorr verification algorithm. Hence, it is sufficient to describe the **Verify** algorithm of the Schnorr signature scheme below.

**Verify\\((M,\sigma=(R,c,z),pk)\\):** This is just the standard Schnorr verify algorithm, which can be publicly run by anyone. It works as follow: 

1. Compute \\(R=g^z \mathsf{pk}^c\\).

2. Compute \\(c'=\mathsf{H}(R||\mathsf{pk}||M)\\).

3. Check if \\(c'=c\\). If the check passes, return \\(1\\), otherwise return \\(0\\).

One can see that, if \\((R,c,z)\\) is a valid Schnorr signature scheme, which has the form \\(R=g^r, c=\mathsf{H}(R||\mathsf{pk}||M)\\\) and \\(s=r+c \cdot \mathsf{sk})\\), then the verification algorithm above returns \\(1\\) since \\(c=c'\\). The converse direction also holds, i.e, if the verify algorithm above return \\(1\\), then \\((R,c,z)\\) must be  a valid Schnorr signature which have the form above.