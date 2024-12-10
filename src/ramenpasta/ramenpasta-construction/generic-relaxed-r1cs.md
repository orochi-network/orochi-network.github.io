# 1. Generic Relaxed R1CS

Generic Relaxed R1CS (GR-R1CS) is essentially a new variant of relaxed R1CS introduced in {{#cite KST22}}, with a segmented structure: its witness is splitted into multiple segments. This flexibility allows us to apply folding schemes that support parallel folding for statements represented in this new variant.

We first recall the syntax of a vector commitment scheme and the notations we will use throughout this section.

## 1.1 A quick tour to vector commitment schemes
<div id="subsubsec:vector-commitment-schemes"></div>

Let $\mathcal{C} = (\text{C.Setup}, \text{C.Commit}, \text{C.Verify})$ be a vector commitment scheme with three key properties: binding, hiding, and additively homomorphic. The three algorithms are defined as follows:
- $\text{ck} \leftarrow \text{C.Setup}(1^\lambda)$: The setup algorithm generates a commitment key $ck$ and determines randomness distribution $\mathrm{R}_{\mathrm{ck}}$, given the security parameter $\lambda$.
- $\tilde{c} \leftarrow \text{C.Commit}(\text{ck}, \mathbf{c}, \hat{\mathbf{c}})$: The commitment algorithm takes as input a commitment key $ck$, a message $\mathbf{c}$ along with a randomness $\hat{\mathbf{c}}$ sampled from $\mathrm{R}_{\mathrm{ck}}$, and returns a commitment $\tilde{c}$.
- $b \leftarrow \text{C.Verify}(\text{ck}, \mathbf{c}, \hat{\mathbf{c}}, \tilde{c})$: The verification algorithm takes as input a commitment key $ck$, a message $\mathbf{c}$, a randomness $\hat{\mathbf{c}}$, and a commitment $\tilde{c}$, and returns a boolean value $b$ indicating whether the commitment is valid.


A tuple $\llbracket \mathbf{c} \rrbracket_{\mathrm{ck}} := (\text{ck}, \tilde{c}, \mathbf{c}, \hat{\mathbf{c}})$ is called a *commitment tuple*.

We define a relation that captures the validity of commitment tuples:
$$
\begin{equation}
  % \label{eq:relation-of-valid-commitment-tuple}
  \mathcal{R}_{\mathrm{com}} = \left\{ \llbracket \mathbf{c} \rrbracket_{\mathrm{ck}} \mid \tilde{c} = \mathrm{C.Commit}(\mathrm{ck}, \mathbf{c}, \hat{\mathbf{c}}) \right\}
\end{equation}
$$

A *vector commitment protocol* allows a prover to commit to a vector $\mathbf{c}$ and send the commitment to a verifier so both parties can check its validity and achieve the commitment tuple $\llbracket \mathbf{c} \rrbracket_{\mathrm{ck}}$.


<div id="eq:vector-commitment-protocol"></div>
$$
\begin{equation}
  % \label{eq:vector-commitment-protocol}
  \mathit{\Pi}_{\mathrm{com}}(\mathrm{ck}; \mathbf{c}) \rightarrow \llbracket \mathbf{c} \rrbracket_{\mathrm{ck}}
\end{equation}
$$

The full specification of the three properties binding, hiding, and additively homomorphic are mentioned in [{{#cite TPN24}}, Appdx. A.6]. Here we describe the notation of the additively homomorphic property, which will be used in the folding schemes. The commitment scheme $\mathcal{C}$ is assumed to be additively homomorphic, which means that for any $\alpha \in \mathbb{F}$ and two commitment tuples $\llbracket \mathbf{c}_0 \rrbracket_{\mathrm{ck}} = (\text{ck}, \tilde{c}_0, \mathbf{c}_0, \hat{\mathbf{c}}_0)$ and $\llbracket \mathbf{c}_1 \rrbracket_{\mathrm{ck}} = (\text{ck}, \tilde{c}_1, \mathbf{c}_1, \hat{\mathbf{c}}_1)$, the following properties hold:
- Additive homomorphism: Let $\mathbf{c}_2 = \mathbf{c}_0 + \mathbf{c}_1$, then $\llbracket \mathbf{c}_2 \rrbracket_{\mathrm{ck}} = (\text{ck}, \tilde{c}_2 = \tilde{c}_0 + \tilde{c}_1, \mathbf{c}_2 = \mathbf{c}_0 + \mathbf{c}_1, \hat{\mathbf{c}}_2 = \hat{\mathbf{c}}_0 + \hat{\mathbf{c}}_1)$. For brevity, we can write $\llbracket \mathbf{c}_2 \rrbracket_{\mathrm{ck}} := \llbracket \mathbf{c}_0 \rrbracket_{\mathrm{ck}} + \llbracket \mathbf{c}_1 \rrbracket_{\mathrm{ck}}$.
- Scalar-multiplicative homomorphism: Let $\mathbf{c}_3 = \alpha \cdot \mathbf{c}_0$, then $\llbracket \mathbf{c}_3 \rrbracket_{\mathrm{ck}} = (\text{ck}, \tilde{c}_3 = \alpha \cdot \tilde{c}_0, \mathbf{c}_3 = \alpha \cdot \mathbf{c}_0, \hat{\mathbf{c}}_3 = \alpha \cdot \hat{\mathbf{c}}_0)$. For brevity, we can write $\llbracket \mathbf{c}_3 \rrbracket_{\mathrm{ck}} := \alpha \cdot \llbracket \mathbf{c}_0 \rrbracket_{\mathrm{ck}}$.

These definitions and properties form the foundation for the constructions of folding schemes in subsequent sections.

## 1.2 Generic Relaxed R1CS (GR-R1CS)

Let $\mathbb{F}$ be a finite field. Let $c, m_{1}, \ldots, m_{c}, n \in \mathbb{Z}_{+}, m=1+\sum_{i=1}^{c} m_{i}$.
  
A GR-R1CS *instance-witness pair* $\mathbb{x} = (\mathbb{x}.u, \mathbb{x}.\mathrm{pub}, \mathbb{x}.\mathbf{z}_{1}, \ldots, \mathbb{x}.\mathbf{z}_{c}, \mathbb{x}.\mathbf{e})$ corresponding to a GR-R1CS structure $\mathfrak{S}=(\mathbf{A}, \mathbf{B}, \mathbf{C}) \in\left(\mathbb{F}^{n \times m}\right)^{3}$ consists of:
- Instance part:
  - A supporting scalar: $u \in \mathbb{F}$
  - A public i/o scalar: $\mathrm{pub} \in \mathbb{F}$
  - An error vector: $\mathbf{e} \in \mathbb{F}^{n}$
- Witness part: $c$ vectors: $\mathbf{z}_{i} \in \mathbb{F}^{m_{i}}$ for $i \in [c]$

Let $\mathrm{tck} = (\mathrm{ck}_{1}, \ldots, \mathrm{ck}_{c}, \mathrm{cke})$ be a tuple of commitment keys. Then, we write a *committed instance-witness pair* $\llbracket \mathbb{x} \rrbracket_{\mathrm{tck}}$ where the witnesses and the error vector are committed using the commitment keys in $\mathrm{tck}$ respectively:
$$
<div id="eq:committed-instance-witness-pair"></div>
\begin{equation}
\llbracket \mathbb{x} \rrbracket_{\mathrm{tck}}=\left(\mathbb{x}.u, \mathbb{x}.\mathrm{pub}, \llbracket \mathbb{x}.\mathbf{z}_{1} \rrbracket_{\mathrm{ck}_{1}}, \ldots, \llbracket \mathbb{x}.\mathbf{Z}_{c} \rrbracket_{\mathrm{ck}}, \llbracket \mathbb{x}.\mathbf{e} \rrbracket_{\mathrm{cke}}\right)
\end{equation}
$$

A committed instance-witness pair $\llbracket \mathbb{x} \rrbracket_{\text{tck}}$ is a valid GR-R1CS pair corresponding to the GR-R1CS structure $\mathfrak{S}$ if it satisfies the relation

<div id="eq:relation-of-valid-GR-R1CS-pair"></div>
$$
\begin{equation}
\mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}}=\left\{
\begin{array}{l|l}
\llbracket \mathbb{x} \rrbracket_{\mathrm{tck}} & \begin{array}{l}
\mathbb{x}.u \in \mathbb{F} \wedge \mathbb{x}.\mathrm{pub} \in \mathbb{F} \wedge\left(\mathbb{x}.\mathbf{z}_{i} \in \mathbb{F}^{m_{i}} \forall i \in[c]\right) \wedge \mathbb{x}.\mathbf{e} \in \mathbb{F}^{n}
\wedge \llbracket \mathbf{z}_{i} \rrbracket_{\mathrm{ck}} \in \mathcal{R}_{\mathrm{com}} \forall i \in[c] \\
\wedge \llbracket \mathbb{x}.\mathbf{e} \rrbracket_{\mathrm{cke}} \in \mathcal{R}_{\mathrm{com}} \\
\wedge \mathbf{A} \cdot \mathbf{z}^{\prime} \circ \mathbf{B} \cdot \mathbf{z}^{\prime}=\mathbb{x}.u \cdot \mathbf{C} \cdot \mathbf{z}^{\prime}+\mathbb{x}.\mathbf{e}
\end{array}
\end{array}\right\}
\end{equation}
$$

where the *solution vector* $\mathbf{z}^{\prime}=\left(\mathbb{x} \cdot \operatorname{pub}\left\|\mathbb{x} \cdot \mathbf{z}_{1}\right\| \ldots \| \mathbb{x} \cdot \mathbf{z}_{c}\right)$ is a vector of size $m$ that is the concatenation of the public input and the witness segments, and $\circ$ is the entry-wise multiplication \footnote{This solution vector is a bit different from the original one of rR1CS, which has the structure $z = (\mathbf{W} \| \mathrm{u} \| \mathrm{x})$, where $\mathbf{W}$ is the witness vector, $\mathrm{u}$ is the supporting scalar, and $\mathrm{x}$ is the public i/o vector.}. We also write this as $\mathbb{x}.\mathbf{z}^{\prime}$. Similar syntaxes also apply for other components in $\mathbb{x}$.

<div id="def:c-GR-R1CS-pair"></div>
Moreover, for convenience, we call $\mathbb{x}$ a $c$-GR-R1CS pair if there are $c$ segments in the witness part of $\mathbb{x}$. Similarly, we also adapt the notation of $\mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}}$ to $\mathcal{R}_{c-\mathrm{GR-R1CS}}^{\mathfrak{S}}$ for the $c$-GR-R1CS relation.

## 1.3 A folding scheme for GR-R1CS

Now, we are ready to construct an extractable folding scheme for GR-R1CS.

**Construction 1: Protocol** $\mathit{\Pi}_{\text{GR-R1CS}}$:

<!-- \label{construction:folding-scheme-for-GR-R1CS} -->

Given two satisfying committed instance-witness pairs $\llbracket \mathbb{x}_{0} \rrbracket_{\text{tck}}$, $\llbracket \mathbb{x}_{1} \rrbracket_{\text{tck}} \in \mathcal{R}_{\text{GR-R1CS}}^{\mathfrak{S}}$. We can fold $\llbracket \mathbb{x}_{0} \rrbracket_{\text{tck}}$ and $\llbracket \mathbb{x}_{1} \rrbracket_{\text{tck}}$ into a satisfying folded pair $\llbracket \mathbb{x} \rrbracket_{\text{tck}} \in \mathcal{R}_{\text{GR-R1CS}}^{\mathfrak{S}}$ using a random linear combination with a randomness $\alpha$.

$\mathit{\Pi}_{\text{GR-R1CS}}\left(\mathrm{tck}=\left(\mathrm{ck}_{1}, \ldots, \mathrm{ck}, \mathrm{cke}\right), \mathfrak{S}, \llbracket \mathbb{x}_{0} \rrbracket_{\text{tck}}, \llbracket \mathbb{x}_{1} \rrbracket_{\text{tck}}\right) \rightarrow \llbracket \mathbb{x} \rrbracket_{\text{tck}}$:

1. Prover: calculate the garb vector $\mathbf{g} \leftarrow \operatorname{garb}\left(\mathfrak{S}, \llbracket \mathbb{x}_{0} \rrbracket_{\text{tck}}, \llbracket \mathbb{x}_{1} \rrbracket_{\text{tck}}\right)$ of size $n$, where
<div id="eq:garb-vector-for-GR-R1CS"></div>
$$
\begin{equation}
\operatorname{garb}\left(\mathfrak{S}, \llbracket \mathbb{x}_{0} \rrbracket_{\mathrm{tck}}, \llbracket \mathbb{x}_{1} \rrbracket_{\mathrm{tck}}\right)=\mathbf{A} \cdot \mathbf{z}_{0}^{\prime} \circ \mathbf{B} \cdot \mathbf{z}_{1}^{\prime}+\mathbf{A} \cdot \mathbf{z}_{1}^{\prime} \circ \mathbf{B} \cdot \mathbf{z}_{0}^{\prime}-\mathbf{C} \cdot\left(\mathbb{x}_{1}.u \cdot \mathbf{z}_{0}^{\prime}+\mathbb{x}_{0}.u \cdot \mathbf{z}_{1}^{\prime}\right)
\end{equation}
$$
in which $\mathbf{z}_{0}^{\prime}:=\mathbb{x}_{0}.\mathbf{z}^{\prime}, \mathbf{z}_{1}^{\prime}:=\mathbb{x}_{1}.\mathbf{z}^{\prime}$

2. Both parties run $\llbracket \mathbf{g} \rrbracket_{\text{cke}} \leftarrow \mathit{\Pi}_{\text{com}}(\mathrm{cke} ; \mathbf{g})$ where $\mathit{\Pi}_{\text{com}}$ is defined in [(#)](#eq:vector-commitment-protocol).

3. Verifier: $\alpha \stackrel{\$}{\leftarrow} \mathbb{F}$ and send $\alpha$ to prover.

4. This step folds $\llbracket x_{0} \rrbracket_{\text{tck}}$ and $\llbracket x_{1} \rrbracket_{\text{tck}}$ with $\alpha$. Specifically, both parties compute: <div id="step-4-folding-GR-R1CS-pair"></div>
   - $\mathbb{x}.u=\mathbb{x}_{0}.u+\alpha \cdot \mathbb{x}_{1}.u$
   - $\mathbb{x}.pub := \mathbb{x}_{0}.pub + \alpha \cdot \mathbb{x}_{1}.pub$
   - $\llbracket \mathbb{x}.\mathbf{z}_{i} \rrbracket_{\mathrm{ck}_{i}}:=\llbracket \mathbb{x}_{0}.\mathbf{z}_{i} \rrbracket_{\mathrm{ck}_{i}}+\alpha \cdot \llbracket \mathbb{x}_{1}.\mathbf{z}_{i} \rrbracket_{\mathrm{ck}_{i}} \forall i \in[c]$. The prover can internally computes the corresponding folded segmented witnesses $\mathbb{x}.\mathbf{z}_i = \mathbb{x}_{0}.\mathbf{z}_{i} + \alpha \cdot \mathbb{x}_{1}.\mathbf{z}_{i} \quad \forall i \in[c]$
   - $\llbracket \mathbb{x}.\mathbf{e} \rrbracket_{\mathrm{cke}}:=\llbracket \mathbb{x}_{0}.\mathbf{e} \rrbracket_{\mathrm{cke}}+\alpha \cdot \llbracket \mathbf{g} \rrbracket_{\mathrm{cke}}+\alpha^{2} \cdot \llbracket \mathbb{x}_{1}.\mathbf{e} \rrbracket_{\mathrm{cke}}$. The prover can internally computes the corresponding folded error vector $\mathbb{x}.\mathbf{e} = \mathbb{x}_{0}.\mathbf{e} + \alpha \cdot \mathbf{g} + \alpha^{2} \cdot \mathbb{x}_{1}.\mathbf{e}$


By some expansion, we can indeed derive the equality for satisfiability of $\llbracket \mathbb{x} \rrbracket_{\text{tck}}$ in this construction
\begin{equation}
\mathbf{A} \cdot \mathbf{z}^{\prime} \circ \mathbf{B} \cdot \mathbf{z}^{\prime}=\mathbb{x} \cdot u \cdot \mathbf{C} \cdot \mathbf{z}^{\prime}+\mathbb{x} \cdot \mathbf{e}\end{equation}

The special soundness property of $\mathit{\Pi}_{\text{GR-R1CS}}$ is elaborated in [{{#cite TPN24}}, Lemma 1]. This property involves rewinding $\mathit{\Pi}_{\text{GR-R1CS}}$ from Construction [1](#a-folding-scheme-for-gr-r1cs) three times, maintaining the same $\tilde{\mathrm{g}}$ (the commitment in $\llbracket \mathbf{g} \rrbracket_{\text{cke}}$) but using three distinct challenges $\left\{\alpha^{(i)}\right\}_{i \in[3]}$ to derive the three folded pairs. From which the original two pairs can be extracted. The detailed proof of this extraction is provided in [{{#cite TPN24}}, Appdx. A.9].

## 1.4 Hierarchical Structures

The IVC problem involves an incremental computation of $N$-step computation. We want to apply the folding scheme in Construction [1](#a-folding-scheme-for-gr-r1cs) to fold the computation in parallel. To do that, we first construct R1CS-based instances for each computation step, then apply the folding scheme to fold these instances in a tree-like structure - which the authors call *hierarchical structure*. This approach allows us to fold the computation in parallel. Below we describe the definition of hierarchical structures.

<!-- \label{def:hierarchical-structures} -->
For any $l, r \in \mathbb{Z}_{+}, l<r$, a family $\mathcal{H S}_{l r}$ is a set of hierarchical structures s.t. each $\mathrm{HS} \in \mathcal{H} \mathcal{S}_{\text{lr}}$ satisfies:

- If $l+1=r$, then HS contains only the root node that represents the folded instance for the whole range $(l, r]$.
- If $l+1<r$, then there exists uniquely $j \in[l+1, r-1]$ s.t. there exist $\mathrm{HS}_{0} \in \mathcal{H S}_{l j}, \mathrm{HS}_{1} \in \mathcal{H} \mathcal{S}_{j r}$ satisfying $\mathrm{HS}=\mathrm{HS}_{0} \cup \mathrm{HS}_{1} \cup\{(l, r]\}$.