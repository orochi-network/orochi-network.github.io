### Elliptic Curves

#### Definition
Let \\(K\\) be a field. An **elliptic curve** \\(E\\) is a plane curve defined over the field \\(K\\) as follows:
$$E(K)=\\{y^2=x^3+ax+b : (x,y) \in K^2\\} \cup \\{\mathcal{O}\\}$$
where \\((a,b) \in K^2\\). The point \\(\mathcal{O}\\) is called the infinity point of the curve. The set \\(E(K)\\) forms an abelian group with identity element \\(\mathcal{O}\\).

In addition, we need the curve to have no cusps, self-intersections, or isolated points. Algebraically, this can be defined by the condition \\(4a^3+27b^2 \neq 0\\) in the field \\(K\\).

The **\\(j-\\) invariant** of an elliptic curve is defined to be \\(-1728\frac{4a^3}{4a^3+27b^2}\\). Two elliptic curves are isomorphic to each other if and only if they have the same \\(j-\\) invariant value.

The **endomorphism ring** of \\(E\\) is denoted \\(End(E)\\). The structure of \\(End(E)\\) can be found in Chapter 3.9 of Silverman's book.

For an integer \\(n\\), we define \\(E[n]=\\{(x,y) \in E(K) | n*(x,y)=\mathcal{O}\\}\\)

Over a field \\(\mathbb{F}_p\\), there are two types of curves: **Ordinary** and **Supersingular**, based on the set \\(E[p]\\). We are interested in studying Supersingular curves, since the isogeny graph on these curves has nice structure and properties. 