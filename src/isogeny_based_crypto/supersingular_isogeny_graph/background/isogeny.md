### Isogenies

#### Definition

Let \\(E_1:y^2=x^3+a_1x+b_1\\) and \\(E_2:y^2=x^3+a_2x+b_2\\) be elliptic curves over a fielđ \\(K\\). An isogeny from \\(E_1\\) to \\(E_2\\) is a nonconstant homorphism \\(\alpha:E_1 \rightarrow E_2\\) that is given by rational functions. 

That means \\(\alpha(P+Q)=\alpha(P)+\alpha(Q)\\) for all \\(P,Q \in E_1\\) and there are rational functions \\(R_1, R_2\\) such that if \\(\alpha(x_1, y_1)=(x_2, y_2)\\) then \\((x_2, y_2)=(R_1(x_1, y_1),R_2(x_1, y_1))\\).

In fact, it can be proved that we can write \\(\alpha\\) in the form \\(\alpha(x_1, y_1)=(r_1(x_1), y_1r_2(x_1))\\).

If \\(r_1(x)=\dfrac{p(x)}{q(x)}\\) for polynomials 
\\(p\\) and \\(q\\) without common roots, define the degree of \\(alpha\\) to be \\(Max(deg(p(x)),deg(q(x)))\\). 

We say an isogeny is seperable if \\(q(x)\\) have no repeated roots.

#### Example

Consider two curves \\(E_1:y^2=x^3+x\\) and \\(E_2:y^2=x^3-4x\\) over \\(\mathbb{F}_{11}\\). Then the map
$$\alpha: E_1 \rightarrow E_2$$
$$(x,y) \rightarrow (\dfrac{x^2+1}{x},\dfrac{y(x^2-1)}{x})$$
is an isogeny from \\(E_1\\) to \\(E_2\\).

#### Properties

We mention several important properties of isogenies.

1. Isogenies are uniquely determined by their kernel: Given an elliptic curve \\(E\\) and a subgroup \\(L\\), there is an unique elliptic curve \\(E'\\) and an isogeny \\(\alpha: E \rightarrow E'\\) such that the kernel of \\(\alpha\\) is \\(L\\).

1. [Sil09, Chapter III.6, Theorem 6.2] For every isogeny \\(\alpha: E \rightarrow E'\\) of degree \\(l\\), there exists an unique dual isogeny \\(\hat{\alpha}: E' \rightarrow E\\) such that \\(\alpha \hat{\alpha}= \hat{\alpha} \alpha=l\\)

1. [Gha21, Proposition 2.2] (Decomposition of isogenies)  Let \\(\alpha: E \rightarrow E'\\) be a seperable isogeny. Then there exists an integer \\(k\\) elliptic curves \\(E=E_0, E_1,...,E_n=E'\\) and isogenies \\(\beta_i: E_i \rightarrow E_{i+1}\\) of prime degree such that \\(\alpha=\beta_{n-1} \beta_{n-2} ... \beta_{0} [k]\\)

The edges of Supersingular isogeny graphs are determined by isogenies between the curves, we will talk about it later in the definition of the graph.