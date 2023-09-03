### Verification

Recall that the verification algorithm in threshold ECDSA remain identical to an ordinary ECDSA verification algorithm. Hence, it is sufficient to describe the **Verify** algorithm of the threshold ECDSA below.

**Verify\\((M,\sigma=(r,s),pk)\\):** This is just the standard ECDSA verify algorithm. It works as follow: 

1. Compute \\(m=\mathsf{H}(M)\\).
2. Check \\(r=(g^m\cdot pk^{r})^{s^{-1}}\\).