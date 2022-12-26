# Security Requirements
In this section, we briefly discuss the security requirement for polynomial commitment schemes. 

A polynomial commitment scheme is said to be secure if it satisfies the following properties:

- **Correctness.** The correctness property says that if the scheme is executed honestly then the verification algorithms, namely, \\(\mathsf{VerifyPoly}\\) and \\(\mathsf{VerifyEval}\\), always returns \\(1\\). In particular, assume that \\(ck \leftarrow \mathsf{Setup}(1^\kappa, t)\\) and \\((c, d) \leftarrow \mathsf{Commit}(ck, f(X))\\). Then, \\(\mathsf{VerifyPoly}(ck, f(X), c, d)\\) always returns \\(1\\). And, for any \\(w_x\\) output by \\(\mathsf{CreateWitness}(ck, f(X), x, d)\\), algorithm \\(\mathsf{VerifyEval}(ck, c, x, f(x), w_x)\\) always returns \\(1\\).

- **Polynomial Binding.** For a given commitment key \\(ck\\) output by \\(\mathsf{Setup}(1^\lambda, t)\\), this property says that it is hard to output commitment \\(c\\) and two tuples \\((f(X), d)\\) and \\((f'(X), d')\\) such that \\(f(X)\\) and \\(f'(X)\\) are distinct and have degrees at most \\(t\\), and \\(c\\) is commitment to both \\(f(X)\\) and \\(f'(X)\\) with respect to openings \\(d\\) and \\(d'\\), respectively.  More precisely, it is hard to make \\(\mathsf{VerifyPoly}(ck, f(X), c, d) = 1\\) and \\(\mathsf{VerifyPoly}(ck, f'(X), c, d') = 1\\) if \\(f(X) \not= f'(X)\\), \\(\deg(f) \leq t\\) and \\(\deg(f') \leq t\\).

- **Evaluation Binding.** The evaluation binding property says that a committed polynomial evaluating on an index \\(x\\) cannot produce two different outcomes \\(v\\) and \\(v'\\). More precisely, an adversary cannot provide an index \\(x\\) and \\(2\\) tuples \\((v, w_x)\\) and \\((v', w'_x)\\) satisfying \\(v \not= v'\\) such that \\(\mathsf{VerifyEval}(ck, c, x, v, w_x) = 1\\) and \\(\mathsf{VerifyEval}(ck, c, x, v', w'_x) = 1\\).

- **Hiding.** An adversary \\(\mathcal{A}\\) who knows at most \\(\deg(f)\\) evaluations of a committed polynomial \\(f(X)\\) cannot determine any evaluation that it does not know before. 
> We remind that knowing \\(\deg(f) + 1\\) different evaluations helps to determine polynomial, and hence all other evaluations. Therefore, we use the bound \\(\deg(f)\\) here as a maxmimum number of evaluations that the adversary is allowed to know in order not to correctly obtain the evaluations of all other indices.