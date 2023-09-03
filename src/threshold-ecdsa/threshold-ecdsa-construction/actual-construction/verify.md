### Verification

Recall that the verification algorithm in threshold ECDSA remain identical to an ordinary ECDSA verification algorithm. Hence, it is sufficient to describe the **Verify** algorithm of the threshold ECDSA below.

**Verify\\((M,\sigma=(r,s),pk)\\):** This is just the standard ECDSA verify algorithm, which can be publicly run by anyone. It works as follow: 

1. Compute \\(m=\mathsf{H}(M)\\).
2. Compute \\(u_1=m\cdot s^{-1}\\) and \\(u_2=r\cdot s^{-1}\\).
3. Compute \\(R=g^{u_1}pk^{u_2}\\).
4. Check if \\(r=R.\mathsf{x}\\). If the check passes, return \\(1\\), otherwise return \\(0\\).

One can see that, if \\((r,s)\\) is a valid ECDSA signature scheme, which has the form \\(r=g^{k^{-1}}.\mathsf{x}\\) and \\(s=k(m+r\cdot sk)\\), then the verification algorithm above returns \\(1\\) since \\(R=g^{u_1}pk^{u_2}=g^{s^{-1}(m+r\cdot sk)}=g^{k^{-1}}\\). 