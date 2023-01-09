# Correct Evaluation from the Commitment
> **Warning.** This part explains by the use of algebra. You may skip if you feel it is complicated.

For an index \\(i\\) given to the committer, since committer knows \\(f(X)\\), he can compute \\(f(i)\\) definitely. The \\(\mathsf{CreateWitness}\\) algorithm is constructed based on the fact that \\(X - i\\) divides \\(f(X) - f(i)\\). At this point, there is something difficult to realize here since it regards to the use of algebra. However, we know that \\(f(i)\\) is the output of \\(f(X)\\) on input \\(i\\). Therefore, we see that \\(i\\) is among the roots of \\(g(X) = f(X) - f(i)\\), i.e., \\(g(i) = 0\\) which says that \\(i\\) is a root of \\(g(X)\\). Therefore, \\(X - i\\) divides \\(f(X) - f(i)\\). Hence, to guarantee the evaluation binding property, committer needs to show that \\(f(X) - f(i)\\) is divisible by \\(X - i\\).

> **Example.** Consider polynomial \\(f(X) = 6X^3 + 25X^2 + 16X + 19\\) in \\( \mathbb{Z}_{31}\\). Let \\(i = 28\\). Then, \\(f(28) = 151779 = 3\\) in \\( \mathbb{Z} _{31} \\). Hence, \\(X - 28 = X + 3\\) divides \\(f(X) - 3\\). In fact, 
> $$ f(X) - 3 = 6X^3 + 25X^2 + 16X + 16 = (3X + 5)(2X + 30)(X + 3)\text{ in } \mathbb{Z} _{31}.$$
> It is obvious that \\(X + 3\\) is a factor of \\(f(X) - 3\\).

Equivalently, we can say that \\(v = f(i)\\) if and only if \\(X - i\\) divides \\(f(X) - f(i)\\).

To show such divisibility holds, we can compute \\(\psi(X) = \frac{f(X) - v_i}{X - i}\\), where \\(v_i\\) is assumed to be \\(f(i)\\), and define witness \\(w_i = g^{\psi(x)}\\) by using \\(g^1, g^x, \dots, g^{x^d}\\) output by the algorithm \\(\textsf{Setup}\\). 

At this point, for the verifier to verify, committer needs to show that \\(\psi(x) \cdot (x - i) + v_i = f(x)\\). Let's closely take a look at this formula. We observe the followings:
1. No one knows \\(x\\). Hence, \\(\psi(x)\\) is not known to anyone. 
2. Committer and verifier know \\(g^{\psi(x)}\\) which is equal to the commitment \\(c\\). Moreover, they also know \\(g^x, g^i, g^{v_i}\\) since \\(g^x\\) belongs to commitment key \\(ck\\), \\(i\\) is public and \\(v_i\\) is publicly claimed by committer.
3. Verifier can easily compute \\(g^{x - i} = g^x / g^i\\).

Clearly, having \\(g^{\psi(x)},g^{x - i}, g^{v_i}\\) and \\(g^{f(x)}\\), we do not know any efficient way to compute \\(g^{\psi(x)\cdot (x - i) + v_i}\\) since computing \\(g^{\psi(x)\cdot (x-i)}\\) is hard due to Diffie-Hellman assumption.

## Using Bilinear Pairing to Handle Correct Multiplications

> Recall the bilinear pairing \\(e : \mathbb{G}\times \mathbb{G} \to \mathbb{G}_T\\) where \\(\mathbb{G}\\) and \\(\mathbb{G}_T\\) are some groups of the same cardinality. This bilinear pairing has \\(2\\) properties: bilinearity and non-degeneracy. However, to avoid confusion, we only care about the bilinearity and temporarily skip the notice to non-degeneracy. 
> - **Bilinearity.**  For \\(g \in \mathbb{G}\\) and \\(g_T \in \mathbb{G}_T\\), \\(e(g^a, g^b) = e(g, g)^{ab}\\) for any \\(a, b \in \mathbb{Z}_p\\) where \\(p=|\mathbb{G}|\\).

The validity of the witness can be check easily by using pairing, namely, $$e(w_i, g^x / g^i)\cdot e(g,g)^{v_i} \stackrel{?}{=}e(c, g),$$ where \\(c\\) is the commitment to \\(f(X)\\).
If the above identity holds, with non-negligible probability, it says that \\(v_i = f(i)\\).

To show identity implies \\(v_i = f(i)\\) with non-negligible probability, we consider \\(w_i = g^{\psi(x)} = g^{\frac{f(x) - v_i}{x - i}}\\). Hence, 
$$
    \begin{align}
        e(w_i, g^x / g^i)\cdot e(g, g)^{v_i} &= e\left(g^{\frac{f(x) - v_i}{x - i}}, g^x / g^i\right)\cdot e(g, g)^{v_i}\\\\
        &= e(g, g)^{f(x) - v_i}\cdot e(g, g)^{v_i} = e(g^{f(x)}, g) = e(c, g).
    \end{align}
$$

Notice that, if the person providing \\(w_i\\) and \\(v_i\\) does not know \\(f(X)\\), then we have the following possibilities:
- This person correctly guesses \\(w_i\\) and \\(v_i\\). This happens with negligible probability if we assumes that field cardinality, namely, number of elements in field \\(\mathbb{F}\\), is large.
- The person incorrectly provides \\(w_i\\) and \\(v_i\\). Specificially, either \\(v_i\\) is not equal to \\(f(i)\\) or \\(w_i\\) is incorrect. Assume that \\(w_i = g^{h_i}\\). This case happens when \\(x\\) is the root of \\(h_i\cdot (X - i) \cdot v_i = f(X)\\). By Schwartz–Zippel lemma, this case also happens with negligible probability if the field cardinality is large and that person does not know \\(x\\), as \\(x\\) at the beginning was assumed to be hidden index.