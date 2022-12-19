# Technical Overview

To discuss the construction, we first sketch its high level idea. We separate the idea into \\(3\\) little parts. 

First, we recall a well-known and long-standing cryptographic assumption named *discrete logarithm*. This assumption helps to guarantee the security of the scheme by applying some black-box reduction, i.e., a technique guaranteeing that, if an adversary can break some security property of the scheme, then we can use that broken piece to find a solution of the discrete logarithm problem. The term ``black-box'' to imply that we do not care about how the adversary does to break some security property of the scheme. Instead, we only care about the result that adversary achieves to solve the hard problem, namely, discrete logarithm problem.

Second, we discuss how to construct a commitment to a polynomial \\(f(X)\\) without explaining hiding property. This part covers the construction of algorithms \\(\mathsf{Setup}, \mathsf{Commit}, \mathsf{VerifyPoly}\\) and the polynomial binding property.

Then, in the third part, we discuss the construction of algorithms that guarantee the correct evaluation of polynomial for a given index. This includes the algorithms \\(\mathsf{CreateWitness}, \mathsf{VerifyEval}\\) and hiding and evaluation binding properties.

## Discrete Logarithm Problem
> There are \\(2\\) things we need to make clear here for anyone who is not familiar with cryptography, namely, cryptographic assumption and discrete logarithm assumption.
>
> In the context of this writing, we can assume that cryptographic assumption is some problem that is hard to solve by using our computation. It means that, to solve such a problem, the computer, or even some super computer or a large number of computers, needs to take a huge amount of time, possibly a few years to million of years, to find a solution for such problem.
>
> And, how about discrete logarithm problem? Well. To discuss this one, we need to understand something like *group theory*. But, as said previously, in the context of this writing, it is not required to know entire group theory. Instead, I will discuss some necessary things about the group required to use in this case. Specificially, it is cyclic group.<span style="color:red"> to be written later</span>

## Commitment to \\(f(X)\\) Without Hiding Property
In the construction of {{#cite KZG10}}, the main idea to commit a polynomial is to evaluate it on a secret index. For example, assume that \\(f(X) = a_0 + a_1 X + \dots + a_d X^d \in \mathbb{F}[X]\\). The secret index can be thought of as some value \\(x\\) that the committer does not know. So, how can committer evaluate \\(f(x)\\) on that secret index without any knowledge about it? In fact, cryptography can magically help you do that. For instance, by putting \\(1, x, x^2, \dots, x^n \\) into the form of powers to some base element \\(g\\), e.g., \\(g^1, g^x, g^{x^2}, \dots, g^d\\), it helps to hide those values \\(1, x, x^2, \dots, x^d\\). Moreover, it alows you to evaluate \\(g^{f(x)}\\) as desired by computing
$$ (g^1)^{a_0} \cdot (g^x)^{a_1} \cdot (g^{x^2})^{a_2} \cdot \dots \cdot (g^{x^d})^{a_d} = g^{a_0 + a_1x + \dots a_d x^d} = g^{f(x)}.$$
Thus, \\(g^{f(x)}\\) is computed without any knowledge about \\(x\\). Hence, that is whatever the committer needs to do in the commit step, namely, executing the algorithm \\(\textsf{Commit}\\) to output the commitment \\(g^{f(x)}\\). So the commitment key \\(ck\\) for this algorithm is the hidden indices wrapped under powers of \\(g\\), namely, the set sequence \\((g^1, g^{x}, g^{x^2}, \dots, g^{x^d})\\). And, therefore, \\((g^1, g^{x}, g^{x^2}, \dots, g^{x^d})\\) is also the output of the algorithm \\(\textsf{Setup}\\). At this point, we might think about a few things:
1. How to verify the commitment \\(c = g^{f(x)}\\) by executing \\(\textsf{VerifyPoly}\\).
2. How to guarantee that the commitment satisfies the polynomial binding property.

For the first question, to verify \\(c\\), the decommitment of the construction is \\(f(X)\\) itself. Committer simply send the entire polynomial \\(f(X)\\) to verifier, namely, by sending coefficients \\(a_0, \dots, a_d\\). Having the polynomial and the commitment key \\(ck\\), the verifier can check easily by repeating steps similar to the algorithm \\(\textsf{Commit}\\). 

For the second question, to show the satisfaction of binding property, we can assume that the committer is able to break the binding property by introducing another polynomial \\(f'(X) = a'_0 + a'_1X + \dots + a'_dX^d\\) where \\(f'(X) \not= f(X)\\). So we have \\(g^{f(x)} = c = g^{f'(x)}\\).

## Correct Evaluations from the Commitment
> Warning: This part explains by the use of algebra. You may skip if you feel it is complicated.

For an index \\(i\\) given the committer, since committer knows \\(f(X)\\), he can compute \\(f(i)\\) definitely. The \\(\mathsf{CreateWitness}\\) algorithm is constructed based on the fact that \\(X - i\\) divides \\(f(X) - f(i)\\). At this point, there is something difficult to realize here since it regards to the use of algebra. However, we know that \\(f(i)\\) is the output of \\(f(X)\\) on input \\(i\\). Therefore, we see that \\(i\\) is among the roots of \\(g(X) = f(X) - f(i)\\), i.e., \\(g(i) = 0\\) which says that \\(i\\) is a root of \\(g(X)\\). Therefore, \\(X - i\\) divides \\(f(X) - f(i)\\). Hence, to guarantee the evaluation binding property, committer needs to show that \\(f(X) - f(i)\\) is divisible by \\(X - i\\).

To show such divisibility holds, <span style="color:red"> to be written later</span>