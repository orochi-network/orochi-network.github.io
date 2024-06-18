### Modular Square Root based Construction

One of the early example of VDF can be found in the paper of Dwork and Naor {{#cite DN92}}. Their construction require a prover, to compute \\(Y=\sqrt(X) \pmod{p}\\) for a prime \\(p\\) and input \\(X\\). When \\(p \equiv 3 \pmod{4}\\) we see that \\(Y=X^{\dfrac{p+1}{4}} \pmod{p}\\), and thus computing \\(Y\\) requires \\(O(\log p)\\) computation steps. Verifying only require one multiplication by checking \\(Y^2 \equiv X \pmod{p}\\). The construction is formally described as follow.

**\\(\mathsf{Gen}(1^\lambda)\\)**: The algorithm outputs a \\(\lambda\\) bit prime \\(p\\) where \\(p \equiv 3 \pmod{4}\\).

**\\(\mathsf{Eval}(X)\\)**: Compute \\(Y=X^{\dfrac{p+1}{4}} \pmod{p}\\). The proof \\(\pi\\) is \\(Y\\).

**\\(\mathsf{Verify}(X,Y,\pi)\\)**: Check if \\(Y^2 \equiv X \pmod{p}\\).

This construction, althought very simple, has two problem: First, the time parameter \\(t\\) is only to \\(\log p\\), thus to increase \\(t\\), a very large prime \\(p\\) is required. Second, it is possible to use parallel processors to speed up in field multiplications.
