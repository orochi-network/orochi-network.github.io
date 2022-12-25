# Technical Overview

To discuss the construction, we first sketch its high level idea. We separate the idea into \\(3\\) little parts. 

First, we recall a well-known and long-standing cryptographic assumption named *discrete logarithm*. This assumption helps to guarantee the security of the scheme by applying some black-box reduction, i.e., a technique guaranteeing that, if an adversary can break some security property of the scheme, then we can use that broken piece to find a solution of the discrete logarithm problem. The term ``black-box'' to imply that we do not care about how the adversary does to break some security property of the scheme. Instead, we only care about the result that adversary achieves to solve the hard problem, namely, discrete logarithm problem.

Second, we discuss how to construct a commitment to a polynomial \\(f(X)\\) without explaining hiding property. This part covers the construction of algorithms \\(\mathsf{Setup}, \mathsf{Commit}, \mathsf{VerifyPoly}\\) and the polynomial binding property.

Then, in the third part, we discuss the construction of algorithms that guarantee the correct evaluation of polynomial for a given index. This includes the algorithms \\(\mathsf{CreateWitness}, \mathsf{VerifyEval}\\) and hiding and evaluation binding properties.
