# Dealing with Hiding
In the previous sections, <span style="color:red"> to be written later</span>, we discussed the high level idea of the construction of algorithms as well as the polynomial and evaluation binding properties. One remaining thing is the hiding property. In {{#cite KZG10}}, the authors proposed \\(2\\) constructions from discrete logarithm assumption, for conditional hiding, and Pedersen commitment, for unconditional hiding.

> **Remark.** We now make clear the \\(2\\) notions here, namely, conditional hiding and unconditional hiding.
>
> Conditional hiding of a commitment \\(c\\) to a polynomial \\(f(X)\\) is the property protecting the polynomial \\(f(X)\\) from being compromised with a condition that some assumption employed is assumed to be hard. Usually, the hardness of the assumption is against probabilistic polynomial-time adversaries. Here, probabilistic polynomial-time adversaries stand for the machines that attack the scheme with limited amount of time, and this amount of time is upper-bounded by a polynomial on input the security parameter given to the scheme. Probabilistic polynomial time is a notion in the theory of computation. If you would like to know more about the detail, we prefer to check some textbooks. For example, we prefer {{#cite Sipser2012-introduction-to-theory-of-computation}} in this blog.
>
> On the other hand, unconditional hiding means that we cannot extract any information about the polynomial behind. For example, if \\(f(X) = a_0 + a_1X + \dots + a_dX^d\\) and \\(r(X) = r_0 + r_1X + \dots + r_dX^d\\), given that \\(r_0, \dots, r_d\\) are independently and uniformly sampled from \\(\mathbb{F}\\), then \\(f(X) + r(X) = (a_0 + r_0) + (a_1 + r_1)X + \dots + (a_d + r_d)X^d\\) completely hides \\(f(X)\\) since \\(a_0 + r_0, a_1 + r_1, \dots, a_d + r_d\\) are uniform in \\(\mathbb{F}\\).

## Conditional hiding from discrete logarithm assumption 
The former, namely, discrete logarithm assumption, guarantees the hiding by its own hardness assumption. In particular, given \\(g\\) and \\(g^v\\) for some secret integer \\(v\\), there is no way to extract any piece of information about \\(v\\) from \\(g\\) and \\(g^v\\) due to hardness of discrete logarithm problem. Therefore, the hiding here is conditional, i.e., discrete logarithm assumption is hard. 

## Unconditional hiding from Pedersen commitment 
The latter, namely, using Pedersen commitment, exploits the use of an additional part to achieve unconditional hiding property, which is secure against powerful adversaries and not limited to PPT ones. Roughly speaking, the external part can be informally thought of as a commitment to a random polynomial with conditional hiding. To perform this, we extend the commitment key \\(ck\\) to contain additional elements \\(h^1, h^x, \dots, h^{x^d}\\) where \\(h\\) is another generator of \\(\mathbb{G}\\) different from \\(g\\). Hence, the commitment key \\(ck\\) now is the tuple \\((g^1, \dots, g^{x^d}, h^1, \dots, h^{x^d})\\). Therefore, whenever committing to a polynomial \\(f(X) = a_0 + a_1X + \dots + a_dX^d\\), we additionally sample a polynomial \\(r(X) = r_0 + r_1X + \dots + r_dX^d\in \mathbb{F}[X]\\). The sampling process can be conducted easily by sampling each \\(r_i\\) uniformly and independently from \\(\mathbb{Z}_{|F|} = \\{0, \dots, |F| - 1\\}\\).

The algorithm \\(\textsf{Commit}\\) now can be evaluated by computing
$$
    c = \prod_{i = 0}^d (g^{x^i})^{a_i} \cdot \prod_{i = 0}^d (h^{x^i})^{r_i} = g^{f(x)}\cdot h^{r(x)},
$$
namely, the original conditional-hiding commitment to \\(f(X)\\) multiplying with the condition-hiding commitment to random polynomial \\(r(X)\\) becomes an unconditional commitment \\(c\\) where auxiliary information \\(\textsf{aux}\\) can be set to be the tuple \\((f(X), r(X))\\). Hence, now, the adversary knows nothing about the evaluations of any index in \\(\mathbb{F}\\). We can see clearly that the commitment \\(c\\) hides \\(f(X)\\) unconditionally since \\(r_0\\) is chosen uniformly from \\(\mathbb{Z}_{|\mathbb{F}|}\\) and, hence, \\((h^{x^0})^{r_0}\\) is uniform in \\(\mathbb{F}\\). It also implies that \\(c\\) is uniform in \\(\mathbb{F}\\).

Since \\(c = g^{f(x)}\cdot h^{r(x)}\\), we can say that \\(c\\) is the multiplication of two parts, namely, the message part \\(g^{f(x)}\\) and the randomness part \\(h^{r(x)}\\).

We now discuss how algorithms \\(\textsf{CreateWitness}\\) and \\(\textsf{VerifyEval}\\) work with respect to introductory of the additional part, namely, \\(h^{r(x)}\\). Then, we discuss the unconditional hiding once at most \\(d\\) evaluations of \\(f(X)\\) with respective witnesses are known.

### Creating witness in unconditional hiding mode
For a given index \\(i\\), the witness output by algorithm \\(\textsf{CreateWitness}\\) is also a multiplication of \\(2\\) parts. We simply call the message evaluation and randomness evaluation parts. 

The message evaluation part is computed identically to the conditional version of the commitment scheme. That is, we compute the formula \\(g^{\psi(x)}\\) where \\(\psi(X) = \frac{f(X) - f(i)}{X - i}\\).

The randomness evaluation part is also conducted similarly. Notice that, since we employ \\(r(X)\\) as a polynomial of degree \\(d\\), we can compute witness for the correct evaluation on the same index \\(i\\), namely, \\(r(i)\\). This randomness evaluation part is equal to \\(h^{\varphi(x)}\\) where \\(\varphi(X) = \frac{r(X) - r(i)}{X - i}\\).

> **Remark.** As previously said, \\(x\\) is unknown to committer. Therefore, the computation of \\(g^{\psi(x)}\\) and \\(h^{\varphi(x)}\\) must depend on the commitment key \\(ck\\) by employing the elements \\(g^1, \dots, g^{x^{d - 1}}, h^1, \dots, h^{x^{d - 1}}\\). Notice that we do not you \\(g^{x^d}\\) and \\(h^{x^d}\\) since \\(\psi(X)\\) and \\(\varphi(X)\\) are polynomials of degrees at most \\(d - 1\\).

The output of algorithm \\(\textsf{CreateWitness}\\) is then equal to \\(w_i = (w^\star_i, s_i)\\) where \\(w^\star_i = g^{\psi(x)} \cdot h^{\varphi(x)}\\) is the multiplication of message evaluation and randomness evaluation parts and \\(s_i = r(i)\\). Notice that \\(r(i)\\) is attached to witness in order to help the evaluation verification algorithm, to be described below, work.

### Verifying correct evaluation in unconditional mode
The evaluation verification algorithm \\(\textsf{VerifyEval}\\) receives as inputs the commitment key \\(ck = (g^1, \dots, g^{x^d}, h^1, \dots, h^{x^d})\\), commitment \\(c\\) to \\(f(X)\\), index \\(i\\), value \\(v_i \in \mathbb{F}\\) assumed to be equal to \\(f(i)\\) and \\(w_i = (w^\star_i, s_i)\\). This algorithm is expected to return \\(1\\) is \\(v_i\\) is equal to \\(f(i)\\) with the use of associated witness \\(w_i\\).

To verify whether \\(v_i = f(i)\\), it is worth to verify both correct computations of \\(f(i)\\) and \\(r(i)\\). More precisely, verifier needs to confirm that \\(v_i = f(i)\\) and \\(s_i = r(i)\\). To this end, again, we imitate the verification process of the conditional hiding case. Hence, we employ again bilinear pairing \\(e : \mathbb{G}\times \mathbb{G} \to \mathbb{G}_T\\) which maps \\((g^a, g^b)\\) to \\(e(g, g)^{ab}\\). However, there is an obstacle here. Observe that, in this pairing, we use only \\(1\\) generator \\(g\\) while our commitment employs \\(2\\) different generators \\(g\\) and \\(h\\) both generating \\(G\\). In order to enable the bilinear pairing, we enforce \\(h = g^\gamma\\) for some hidden \\(\gamma\\). This enforcement works because \\(g\\) is the generator of \\(\mathbb{G}\\) and \\(h\\) belongs to \\(\mathbb{G}\\). Hence, our commitment \\(c\\) can be alternatively written as 
$$
    c = g^{f(x)}\cdot h^{r(x)} = g^{f(x)}\cdot g^{\gamma \cdot r(x)} = g^{f(x) + \gamma\cdot r(x)}
$$
which is a conditional commitment to \\(f(X) + \gamma\cdot r(X)\\) with \\(\gamma\\) unknown to both committer and verifier.

Moreover, the witness \\(w^\star_i = g^{\psi(x)}\cdot h^{\varphi(x)}\\) can also be interpreted as 
$$
    w^\star_i = g^{\psi(x)}\cdot h^{\varphi(x)}=g^{\psi(x)} \cdot g^{\gamma\cdot \varphi(x)} = g^{\psi(x) + \gamma\cdot \varphi(x)}
$$ 
which is also a witness for correct evaluation at index \\(i\\) with respect to polynomial \\(f(X) + \gamma\cdot r(X)\\) whose \\(\gamma\\) is not known to both parties, namely, committer and verifier.

We now observe that \\(\psi(X) + \gamma\cdot \varphi(X) = \frac{f(X) - f(i)}{X - i} + \gamma\cdot \frac{r(X) - r(i)}{X - i}\\). Hence, it is worth to guarantee the following equation holds:
$$
    \left(\frac{f(X) - f(i)}{X - i} + \gamma\cdot \frac{r(X) - r(i)}{X - i}\right)\cdot\left(X - i\right) - (f(i) + \gamma\cdot r(i)) = f(X) + \gamma \cdot r(X).
$$

We now describe the process for verifying the above equation by employing the bilinear pairing function \\(g : \mathbb{G}\times \mathbb{G} \to \mathbb{G}_T\\). Since the above equation has a multiplication, we apply the bilinear pairing by checking 
$$
    e\left(g^{\frac{f(x) - v_i}{x - i} + \gamma\cdot \frac{r(x) - s_i}{x - i}}, g^{x - i}\right)\cdot e\left(g^{-\left(v_i + \gamma\cdot s_i\right)}, g\right) = e\left(g^{f(x) + \gamma\cdot r(x)},g\right)
$$
where \\(x\\) and \\(\gamma\\) are unknown to both parties. Since \\(x\\) and \\(\gamma\\) are not known to both parties, it is inconvenient to evaluate the bilinear pairing function. However, since \\(ck = (g^1, \dots, g^{x^d}, h^1, \dots, h^{x^d})\\) and \\(h = g^\gamma\\) are public inputs, we can replace the appearances of \\(x\\) and \\(\gamma\\) in the above equation by those of \\(ck\\) and \\(h\\). The above computation of bilinear pairing hence becomes
$$
    e\left(w^\star, g^x / g^i\right)\cdot e\left(g^{-v_i}\cdot h^{-s_i}, g\right) = e(c, g)
$$
since \\(c = g^{f(x)}\cdot h^{r(x)} = g^{f(x) + \gamma\cdot r(x)}\\).
### Unconditional hiding once at most \\(d\\) evaluations are known
We now assume that the committer allows the adversary to know \\(d\\) different evaluations on \\(f(X)\\), i.e., the evaluations are
$$ \\{(i_1, f(i_1), w_1), \dots, (i_d, f(i_d), w_d)\\}$$
where, for each \\(j \in \\{1, \dots, d\\}\\), \\(\textsf{VerifyEval}(ck, c, i_j, f(i_j), w_j) = 1\\) and \\(w_j = (w_j^\star, s_j)\\).

As we discussed previously, if we know at least \\(d + 1\\) evaluations of \\(f(X)\\) of distinct indices, we can recover \\(f(X)\\) entirely, e.g., by using Lagrange interpolation. On the other hand, knowing evaluations of at most \\(d\\) different indices is insufficient for predicting evaluations of other indices. The unrevealed evaluation of any index, other than those \\(d\\) different indices, would look uniform in \\(\mathbb{F}\\). Hence, since adversary knows at \\(d + 1\\) evaluations of \\(f(X)\\) and the corresponding witnesses \\(w_1, \dots, w_d\\), adversary also knows \\(d + 1\\) different evaluations of \\(r(X)\\). Assume that \\(i_{d + 1} \not\in \\{i_1, \dots, i_d\\}\\). We argue that \\(f(i_{d + 1})\\) and \\(r(i_{d + 1})\\) are unknown to adversary. In fact, assume that there exist \\(f'(X) \not= f(X)\\) and \\(r'(X) \not= r(X)\\) satisfying
$$
    f'(i_j) = f(i_j) = v_j \text{ and } r'(i_j) = r(i_j) = s_j 
$$
for all \\(j \in \\{1, \dots, d\\}\\). Roughly speaking, \\(f'(X)\\) is different from \\(f(X)\\) yet shares the same evaluations at those queried indices, namely, \\(i_1, \dots, i_d\\). A similar explanation applies to \\(r'(X)\\) and \\(r(X)\\). Hence, we argue that \\(w_1, \dots, w_d\\) are the same both pairs \\((f(X), r(X))\\) and \\((f'(X), r'(X))\\) for \\(x\\) fixed in advance. In fact, for any \\(j \in \\{1, \dots, d\\}\\), let's consider the equation described above, with \\(X\\) and \\(i\\) replaced by \\(x\\) and \\(i_j\\), respectively, 
$$
    \left(\frac{f(x) - f(i_j)}{x - i_j} + \gamma\cdot \frac{r(x) - r(i_j)}{x - i_j}\right)\cdot\left(x - i_j\right) - (f(i_j) + \gamma\cdot r(i_j)) = f(x) + \gamma \cdot r(x).
$$
We see clearly that \\(\frac{f(x) - f(i_j)}{x - i_j} + \gamma\cdot \frac{r(x) - r(i_j)}{x - i_j}\\) is uniquely determined by computing 
$$\begin{align}
\frac{f(x) + \gamma\cdot r(x) + f(i_j) + \gamma\cdot r(i_j)}{x - i_j} \&= \frac{f(x) - \gamma\cdot r(x) + v_j + \gamma\cdot s_j}{x - i_j}\\\\
\&= \frac{f'(x) + \gamma\cdot r'(x) + f(i_j) + \gamma\cdot r(i_j)}{x - i_j}
\end{align}$$<span style="color:red">continue here</span>