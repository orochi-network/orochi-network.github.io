### Supersingular Elliptic Curves

#### Definition

Let \\(p\\) is a prime and let \\(q\\) be a power of \\(p\\). Let \\(E\\) be an elliptic curve over \\(\mathbb{F}_q\\). If \\(E[p]=O\\), then \\(E\\) is a **Supersingular elliptic curve**, if \\(E[p]=\mathbb{Z}/p\mathbb{Z}\\) then \\(E\\) is an **Ordinary elliptic curve**.

#### Example

For \\(p=3\\), the curve \\(E: y^2=x^3-x\\) is supersingular over the field \\(\bar{F}_3\\). [Here](https://math.stackexchange.com/questions/3607389/find-all-points-order-3-on-an-elliptic-curve) we see that \\([3]*(x,y)=O\\) for \\((x,y) \neq O\\) if and only if \\(3x^4-6x^2-1=0\\), but such \\(x\\) does not exist since \\(\bar{F}_3\\) has characteristic \\(3\\). Thus \\(E[3]=O\\)

#### Properties

**Theorem** [{{#cite Sil09}}, Chapter V.3, Theorem 3.1] These following conditions are equivalent:

1. \\(E[p^r]=0\\) for all \\(r \geq 1\\).

1. \\(End(E)\\) is an order in a quaternion algebra.

1. The map \\([p]: E \rightarrow E\\) is purely inseperable and \\(j(E) \in \mathbb{F}_{p^2}\\).

As we see, all Supersingular elliptic curves are isomorphic to a curve in \\(F_{p^2}\\), up to isomorphism, therefore the number of these curves are finite. It is natural that we want to count the number of these curves. Fortunately, we have a formula for the number of supersingular elliptic curves, as stated below:

**Theorem**. [{{#cite Sil09}}, Chapter V.4, Theorem 4.1] The number of supersingular elliptic curves up to isomorphism is \\(\left\lfloor \dfrac{p}{12} \right\rfloor+z\\), where 

- \\(z=0\\) if \\(p \equiv 1 \pmod{ 12}\\)

- \\(z=1\\) if \\(p \equiv 5,7 \pmod{ 12}\\)

- \\(z=2\\) if \\(p \equiv 11 \pmod{ 12}\\)

In the next chapter, we introduce the graph where the vertices are the Supersingular elliptic curves (up to isomorphism). This graph has several interesting properties that make it a candidate for constructing post quantum cryptosystems.