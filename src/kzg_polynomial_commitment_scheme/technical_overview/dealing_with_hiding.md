# Dealing with Hiding
The previous sections, <span style="color:red"> to be written later</span>, we discussed the high level idea of the construction of algorithms as well as the polynomial and evaluation binding properties. One remaining thing is the hiding property. In {{#cite KZG10}}, the authors proposed \\(2\\) constructions from discrete logarithm assumption, for conditional hiding, and Pedersen commitment, for unconditional hiding.

> **Remark.** We now make clear the \\(2\\) notions here, namely, conditional hiding and unconditional hiding.
>
> Conditional hiding of a commitment \\(c\\) to a polynomial \\(f(X)\\) is the property protecting the polynomial \\(f(X)\\) from being compromised with a condition that some assumption employed is assumed to be hard. Usually, the hardness of the assumption is against probabilistic polynomial-time adversaries. Here, probabilistic polynomial-time adversaries stand for the machines that attack the scheme with limited amount of time, and this amount of time is upper-bounded by a polynomial on input the security parameter given to the scheme. Probabilistic polynomial time is a notion in the theory of computation. If you would like to know more about the detail, we prefer to check some textbooks. For example, we prefer {{#cite Sipser2012-introduction-to-theory-of-computation}} in this blog.
>
> On the other hand, unconditional hiding means that we cannot extract any information about the polynomial behind. For example, if \\(f(X) = a_0 + a_1X + \dots + a_dX^d\\) and \\(r(X) = r_0 + r_1X + \dots + r_dX^d\\), given that \\(r_0, \dots, r_d\\) are independently and uniformly sampled from \\(\mathbb{F}\\), then \\(f(X) + r(X) = (a_0 + r_0) + (a_1 + r_1)X + \dots + (a_d + r_d)X^d\\) completely hides \\(f(X)\\) since \\(a_0 + r_0, a_1 + r_1, \dots, a_d + r_d\\) are uniform in \\(\mathbb{F}\\).

## Conditional hiding from discrete logarithm assumption 
The former, namely, discrete logarithm assumption, guarantees the hiding by its own hardness assumption. In particular, given \\(g\\) and \\(g^v\\) for some secret integer \\(v\\), there is no way to extract any piece of information about \\(v\\) from \\(g\\) and \\(g^v\\) due to hardness of discrete logarithm problem. Therefore, the hiding here is conditional, i.e., discrete logarithm assumption is hard. 

## Unconditional hiding from Pedersen commitment 
The latter, namely, using Pedersen commitment, exploits the use of an additional part to achieve unconditional hiding property, which is secure against powerful adversaries and not limited to PPT ones. Roughly speaking, the external part can be informally thought of as a commitment to a random polynomial with conditional hiding. To perform this, we extend the commitment key \\(ck\\) to contain additional elements \\(h^1, h^x, \dots, h^{x^d}\\) where \\(h\\) is another generator different from \\(\mathbb{G}\\). Hence, the commitment key \\(ck\\) now is the tuple \\((g^1, \dots, g^{x^d}, h^1, \dots, h^{x^d})\\). Therefore, whenever committing to a polynomial \\(f(X) = a_0 + a_1X + \dots + a_dX^d\\), we additionally sample a polynomial \\(r(X) = r_0 + r_1X + \dots + r_dX^d\in \mathbb{F}[X]\\). The sampling process can be conducted easily by sampling each \\(r_i\\) uniformly and independently from \\(\mathbb{Z}_{|F|} = \\{0, \dots, |F| - 1\\}\\).

The algorithm \\(\textsf{Commit}\\) now can be evaluated by computing
$$
    c = \prod_{i = 0}^d (g^{x^i})^{a_i} \cdot \prod_{i = 0}^d (h^{x^i})^{r_i} = g^{f(x)}\cdot h^{r(x)},
$$
namely, the original conditional-hiding commitment to \\(f(X)\\) multiplying with the condition-hiding commitment to random polynomial \\(r(X)\\) becomes an unconditional commitment \\(c\\) where auxiliary information \\(\textsf{aux}\\) can be set to be the tuple \\((f(X), r(X))\\). Hence, now, the adversary knows nothing about the evaluations of any index in \\(\mathbb{F}\\). We can see clearly that the commitment \\(c\\) hides \\(f(X)\\) unconditionally since \\(r_0\\) is chosen uniformly from \\(\mathbb{Z}_{|\mathbb{F}|}\\) and, hence, \\((h^{x^0})^{r_0}\\) is uniform in \\(\mathbb{F}\\). It also implies that \\(c\\) is uniform in \\(\mathbb{F}\\).

Since \\(c = g^{f(x)}\cdot h^{r(x)}\\), we can say that \\(c\\) is the multiplication of two parts, namely, the message part \\(g^{f(x)}\\) and the randomness part \\(h^{r(x)}\\).

We now discuss how algorithms \\(\textsf{CreateWitness}\\) and \\(\textsf{VerifyEval}\\) work with respect to introductory of the additional part, namely, \\(h^{r(x)}\\).

### Creating witness in unconditional hiding mode
For a given index \\(i\\), the witness output by algorithm \\(\textsf{CreateWitness}\\) is also a multiplication of \\(2\\) parts. We simply call the message evaluation and randomness evaluation parts. 

The message evaluation part is computed identically to the conditional version of the commitment scheme. That is, we compute the formula \\(g^{\psi(x)}\\) where \\(\psi(X) = \frac{f(X) - v_i}{X - i}\\).

The randomness evaluation part is also conducted similarly. Notice that, since we employ \\(r(X)\\) as a polynomial of degree \\(d\\), we can compute witness part for the correct evaluation on the same index <span style="color:red"> to be written later, some error happened with the use of \\(x\\), please check again the previous section too</span>. 

### Verifying correct evaluation in unconditional mode
We now assume that the committer allows the adversary to know \\(d\\) different evaluations on \\(f(X)\\), i.e., the evaluations are
$$ \\{(i_1, f(i_1), w_{i_1}), \dots, (i_d, f(i_d), w_{i_d})\\}$$
where, for each \\(j \in \\{1, \dots, d\\}\\), \\(\textsf{VerifyEval}(ck, c, i_j, f(i_j), w_j) = 1\\).<span style="color:red"> to be written later</span>