### Verification

We describe the **Verify** algorithm of tbe threshold ECDSA below.

**Verify\\((msg,(\rho,\sigma),pk)\\):** This is just the standard ECDSA verify algorithm. It works as follow: 

- Compute \\(m=\mathsf{H}(msg)\\).
- Check \\(\rho=(g^m\cdot pk^{\rho})^{\sigma^{-1}}\\).