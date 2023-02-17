### Supersingular Isogeny Graphs (Pizer Graphs)

#### Definition

Let \\(p\\) be a prime number. For a prime \\(l \neq p\\), a **Supersingluar isogeny graph**  \\(\mathcal{G} _l (\mathbb{F} _{p ^2})\\) is a graph whose vertices are the j-invariants of supersingular elliptic curves in \\(\mathbb{F} _{p ^2}\\), and such that there is an edge from \\(j(E _1)\\) to \\(j(E _2)\\) if and only if there is an isogeny of degree \\(l\\) from \\(E_1\\) to \\(E_2\\). There can be multiple edges from \\(j(E_1)\\) to \\(j(E_2)\\), the number of such edges is equal to the number of isogenies from  \\(j(E_1)\\)  to \\(j(E_2)\\).

Since each vertex represents a supersingular elliptic curve, the number of vertices in \\(\mathcal{G} _l (\mathbb{F} _{p ^2})\\) is equal to \\(\lfloor \frac{p}{12} \rfloor + \epsilon\\), where \\(\epsilon\\) is defined in.

For these graph, we require \\(p \equiv 1 \pmod{ 12} \\) so that we can pair an isogeny with its dual to make the graph regular and undirected {{#cite Gha21}}. 

#### Properties

The reason why Supersingular Isogeny Graphs are important lies in the following theorems:

**Theorem**. [CGL09, Theorem 4.1] For \\(p \equiv 1 \pmod{ 12} \\) and \\(l \neq p\\) , the graph \\(\mathcal{G} _l(\mathbb{F} _{p^2})\\) is connected, and is a \\(l+1\\) regular graph.

**Theorem**. [CGL09, Theorem 4.2] For \\(p \equiv 1 \pmod{ 12} \\) and \\(l \neq p\\) the graph  \\(\mathcal{G} _l(\mathbb{F} _{p^2})\\) are Ramanujan graphs.

We give an overview about Ramanujan graphs. They are optimal expander graph. There are two nice properties of this type of graph. First, relatively short walk on this graph approximate the uniform distribution, which is good for a source of randomness. This can be seen by the following theorem:

**Theorem**. Let \\(N_{l,p}\\) denote the number of vertices of \\(\mathcal{G} _l(\mathbb{F} _{p ^2})\\) .Fix a supersingular \\(j_1 \in \mathbb{F} _{p^2}\\), and let \\(j_2\\) be the endpoint of a walk of length \\(e\\) orginating at \\(j_1\\). Then for all \\(j \in \mathbb{F} _{p^2}\\):

$$|Pr[j=j_2]-N_{l,p}^{-1}| \leq \left(\dfrac{2\sqrt{l}}{l+1}\right)^e$$

The other nice property of Ramanujan graph is that the path-finding problem is assumed to be hard on this graph, which is good for constructing a cryptographic hash function. Two types of Ramanujan are proposed in {{#cite CGL06}}, LPS Graph and Supersingular Isogeny Graphs. However, the LPS hash function was attacked and broken in 2008 {{#cite TZ08, PLQ08}}, leaving the Supersingular Isonegy Graph as the ruling graph. 

The adjacency matrix of \\(\mathcal{G} _l(\mathbb{F} _{p^2})\\) is the Brandt matrix \\(B(l)\\). More informations of the matrix can be found in Voight's book. The matrix allow us to specify all primes \\(p\\) so that the graph does not have short cycles {{#cite Gha21}}, an important property to ensure the hardness of the path-finding problem of the graph ((#cite CGL06)).

