### Verification

Recall that the verification algorithm in threshold ECDSA remain identical to an ordinary ECDSA verification algorithm. Hence, it is sufficient to describe the **Verify** algorithm of the threshold ECDSA below.

**Verify\\((msg,(\rho,\sigma),pk)\\):** This is just the standard ECDSA verify algorithm. It works as follow: 

- Compute \\(m=\mathsf{H}(msg)\\).
- Check \\(\rho=(g^m\cdot pk^{\rho})^{\sigma^{-1}}\\).