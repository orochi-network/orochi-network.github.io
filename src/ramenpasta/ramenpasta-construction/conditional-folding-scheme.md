# 2. Conditional Folding Scheme

The original Nova folding scheme in {{#cite KST22}} is a recursive folding scheme which does not support parallel folding, therefore the folding process of the computation steps must also be an incremental process. To support parallel folding, we need to design a folding scheme that supports verifying the i/o connections between two folded pairs. Specifically, if we want to fold an instance corresponding to a HS in $\mathcal{H S}_{i j}$ and an instance corresponding to a HS in $\mathcal{H S}_{j k}$ where $i < j < k$, we need to verify the connection between the two instances: the output of the first instance must be equal to the input of the second instance.

RAMenPaSTA {{#cite TPN24}} tweaks the Nova folding scheme in two main aspects:
1. Each folded instance carries information of the computation in a contiguous half-open range of steps $(l, r]$ instead of the whole computation from the first step $[1, r]$.
2. The folded instances do not verify the condition of i/o connection between two folded instances. Instead, these conditions are only accumulated in the folding process and their verification is deferred until a final check at the end.

This new scheme is called *Conditional Folding* scheme. In this section, we describe the construction of a generic CF scheme $\mathcal{C} \mathcal{F}_{\text{gnr}}$.

## 2.1 An augmented form of instance-witness pair that supports parallel folding

As usual in the folding schemes, we have a base *computation R1CS* structure for checking the computation step, which is wrapped inside an *augmented R1CS* structure which supports the i/o connection verification. In the conditional folding scheme, we also need such an augmented form of instance-witness pair to do the i/o connection accumulation but not verification. In Definition [1](#def:cf_instance_witness_pair) below, we define this augmented form of instance-witness pair for the conditional folding scheme, which we briefly calls *CF pair*. A CF pair basically contains two folded pairs: one for computation steps and one for i/o connection accumulation. The former is a $5$-GR-R1CS pair and the latter is a $3$-GR-R1CS pair (we follow the notation defined in [#](./generic-relaxed-r1cs.html#def:c-GR-R1CS-pair).

  <!-- \label{def:cf_instance_witness_pair} -->

<div id="def:cf_instance_witness_pair"></div>
$\textbf{Definition 1: Conditional Folding Pair (CF Pair)}$: Let $m_1, \ldots, m_5, n, m_1^{\prime}, m_2^{\prime}, m_3^{\prime}, n^{\prime} \in \mathbf{Z}_{+}$. Let $m=1+\sum_{i=1}^5 m_i$ and $m^{\prime}=1+\sum_{i=1}^3 m_i^{\prime}$.
  
For an $N$-step computation, let $\mathfrak{S}=(\mathbf{A}, \mathbf{B}, \mathbf{C}) \in\left(\mathbb{F}^{n \times m}\right)^3$ and $\mathfrak{S}^{\prime}=\left(\mathbf{A}^{\prime}, \mathbf{B}^{\prime}, \mathbf{C}^{\prime}\right) \in\left(\mathbb{F}^{n^{\prime} \times m^{\prime}}\right)^3$ such that:
- $\mathfrak{S}$ contains R1CS matrices for constraining the witness of a satisfying computation step
- $\mathfrak{S}^{\prime}$ has R1CS matrices for capturing i/o conditions between consecutive steps

Let $\mathcal{C}$ be a homomorphic commitment scheme as defined in [#](./generic-relaxed-r1cs.html#subsubsec:vector-commitment-schemes). We define a CF pair $\llbracket\mathrm{z}\rrbracket_{\mathrm{pp}}$ as follows:


$$\begin{equation}
  \begin{aligned}
  \text{pp}=\left(\quad \text{tck,} \quad \text{tck}^{\prime}\right)
  \end{aligned}
\end{equation}
$$
$$
\begin{equation}
  \begin{aligned}
  \mathbb{z}=\left(\mathrm{lt}, \quad \mathrm{rt}, \mathbb{x}, \quad \mathbf{i}, \quad \mathbf{o}, \quad \mathbb{x}^{\star}, \quad \mathbf{s}, \quad \mathbf{a}\right) \text{,} \\
  \end{aligned}
\end{equation}
$$

<div id="eq:structure-of-valid-CF-pair"></div>
$$
\begin{equation}
  \begin{aligned}
  & \llbracket \mathrm{z} \rrbracket_{\mathrm{pp}}=\left(\begin{array}{llllll}
  \mathrm{lt}, & \mathrm{rt}, & \llbracket \mathbf{x} \rrbracket_{\mathrm{tck}}, \quad & \llbracket \mathbf{i} \rrbracket_{\mathrm{ck}_1}, \quad \llbracket \mathbf{o} \rrbracket_{\mathrm{ck}_2}, \quad \llbracket \mathbf{x}^{\star} \rrbracket_{\mathrm{tck}^{\prime}}, \quad \llbracket \mathbf{s} \rrbracket_{\mathrm{ck}_5}, \quad \llbracket \mathbf{a} \rrbracket_{\mathrm{ck}_5}
  \end{array}\right)
  \end{aligned}
\end{equation}
$$

where
- $\mathrm{pp}$ is the public parameters
- $\mathbb{z}$ is the CF pair and $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$ is the committed CF pair corresponding to the public parameters $\mathrm{pp}$
- $\mathrm{tck} = \left(\mathrm{ck}_1, \ldots, \mathrm{ck}_5, \mathrm{cke}\right)$ and $\mathrm{tck}^{\prime}=\left(\mathrm{ck}_1^{\prime}, \ldots, \mathrm{ck}_3^{\prime}, \mathrm{cke}^{\prime}\right)$ contain commitment keys for committing message vectors over $\mathbb{F}$ of lengths $m_1, \ldots, m_5, n, m_1^{\prime}, m_2^{\prime}, m_3^{\prime}, n^{\prime}$, respectively.
- $\mathrm{lt}, \mathrm{rt} \in \mathbb{N}$ are indices which represent the range $(\mathrm{lt}, \mathrm{rt}]$ of computation steps encapsulated in $\mathbb{z}$
- $\mathbf{i} \in \mathbb{F}^{m_1}$ which contains the input of the computation step (i.e. input at step $\mathrm{lt} + 1$). Similarly, $\mathbf{o} \in \mathbb{F}^{m_2}$ is a vector over $\mathbb{F}$ which contains the output of the computation step (i.e. output at step $\mathrm{rt}$) <span id="eq:io-data-components"></span>
- $\mathbf{s}, \mathbf{a} \in \mathbb{F}^{m_5}$ are vectors over $\mathbb{F}$ which serve as the supporting information for the i/o connection accumulation process (besides the explicit pair $\mathbb{x}^{\star}$ for this purpose). Specifically, $\mathbf{s}$ holds the accumulated sum of all the individual values $\mathbf{s}$ of the CF pairs representing single steps that constitute the computation range $(\mathrm{lt}, \mathrm{rt}]$ and $\mathbf{a}$ holds the randomly accumulated combination of these individual values $\mathbf{s}$ <span id="eq:io-connection-components"></span>
- The step computation pair $\mathbb{x} \in \mathbb{F}^{m}$ and the i/o connection pair $\mathbb{x}^{\star} \in \mathbb{F}^{m^{\prime}}$ are satisfying GR-R1CS pairs ($\mathbb{x} \in \mathcal{R}_{5-\mathrm{GR-R1CS}}^{\mathfrak{S}}, \mathbb{x}^{\star} \in \mathcal{R}_{3-\mathrm{GR-R1CS}}^{\mathfrak{S}^{\prime}}$, with relation syntax from [#](./generic-relaxed-r1cs.html#eq:relation-of-valid-GR-R1CS-pair) for the computation step verification and i/o connection accumulation, respectively. The structure of these two pairs are defined as follows:
  $$\begin{equation}
    \mathbb{x} \quad=\left(\mathbb{x} . u, \quad \mathbb{x} . \mathrm{pub} \quad \mathbb{x} . \mathbf{z}_1, \quad \ldots, \quad \mathbb{x} . \mathbf{z}_5, \quad \text{x.e}\right)
  \end{equation}
  $$
  <div id="eq:structure-of-valid-computation-pair"></div>
  $$\begin{equation}
    \llbracket \mathbb{x} \rrbracket_{\mathrm{tck}}=\left(\quad \llbracket \mathbb{x} . \mathbf{z}_1 \rrbracket_{\mathrm{ck}_1}, \quad \ldots, \quad \llbracket \mathbb{x} . \mathbf{z}_5 \rrbracket_{\mathrm{ck}_5}, \quad \llbracket \mathbb{x} . \mathbf{e} \rrbracket_{\mathrm{cke}}\right)
  \end{equation}
  $$
  $$\begin{equation}
    \mathbb{x}^{\star} \quad=\quad\left(\mathbb{x}^{\star}.u, \quad \mathbb{x}^{\star}.\mathrm{pub}, \quad \mathbb{x}^{\star}. \mathbf{z}_1, \quad \ldots, \quad \mathbb{x}^{\star}. \mathbf{z}_3, \quad \mathbb{x}^{\star}. \mathbf{e}\right)
  \end{equation}
  $$
  <div id="eq:structure-of-valid-io-connection-pair"></div>
  $$\begin{equation}
    \llbracket \mathbb{x}^{\star} \rrbracket_{\mathrm{tck}}=\left(\quad \llbracket \mathbb{x}^{\star}. \mathbf{z}_1 \rrbracket_{\mathrm{ck}_1^{\prime}}, \quad \ldots, \quad \llbracket \mathbb{x}^{\star}. \mathbf{z}_3 \rrbracket_{\mathrm{ck}_3^{\prime}}, \quad \llbracket \mathbb{x}^{\star}. \mathbf{e} \rrbracket_{\mathrm{cke}^{\prime}}\right)
  \end{equation}
  $$
- <div id="eq:relation-between-first-two-ck-in-CF-fair"></div>A deliberate design choice here is that $\mathrm{ck}_1^{\prime} = \mathrm{ck}_2$ and $\mathrm{ck}_2^{\prime} = \mathrm{ck}_1$. This is because the input component $\mathbf{i}$ of $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$ is committed using $\mathrm{ck}_1$ and the output component $\mathbf{o}$ of $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$ is committed using $\mathrm{ck}_2$, and the two components also appear in $\mathbb{x}^{\star}$ but in reversed order ($\mathbf{o}$ comes before $\mathbf{i}$). Therefore, we reuse the same two commitment keys for these two components to save the computation cost of $\llbracket \mathbb{x}^{\star} \rrbracket_{\mathrm{tck}^{\prime}}$.

## 2.2 Validity relations for CF pairs

Besides the base relations $\mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}}$ and $\mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}^{\prime}}$ for the GR-R1CS satisfiability of the two pairs $\mathbb{x}$ and $\mathbb{x}^{\star}$, we also need to define relations that capture the semantic validity of the folding process which involves other components of $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$.

First, we define relation $\mathcal{R}_{\mathrm{gnr-com}}$ to capture the relationship between components in $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$ (i.e. the components are committed using the commitment keys specified in $\mathrm{pp}$):

$$
\begin{equation}
\mathcal{R}_{\mathrm {gnr-com}}=\left\{\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} \mid \llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} \wedge\left(\llbracket \mathbf{i} \rrbracket_{\mathrm{ck}_{1}}, \llbracket \mathbf{o} \rrbracket_{\mathrm{ck}_{2}}, \llbracket \mathbf{s} \rrbracket_{\mathrm{ck}_{5}}, \llbracket \mathbf{a} \rrbracket_{\mathrm{ck}_{5}} \in \mathcal{R}_{\mathrm{com}}\right)\right\} 
\end{equation}
$$

Next, we define a restrictive relation $\mathcal{R}_{\mathrm {gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}$ that is a subset of $\mathcal{R}_{\mathrm {gnr-com}}$ (i.e. the components are committed using the commitment keys specified in $\mathrm{pp}$ plus the two GR-R1CS pairs are satisfied), which specifies the CF pairs with valid structure:

$$
\begin{equation}
\mathcal{R}_{\mathrm{gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}=\left\{\begin{array}{l|l}
\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} & \begin{array}{l}
\mathrm{lt}, \mathrm{rt} \in \mathbb{N} \wedge(\mathrm{lt}<\mathrm{rt}) \wedge \llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} \in \mathcal{R}_{\mathrm{gnr-com}} \\
\wedge \llbracket x \rrbracket_{\mathrm{tck}} \in \mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}} \wedge \llbracket x^{\star} \rrbracket_{\mathrm{tck}} \in \mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}^{\prime}}
\end{array}
\end{array}\right\}
\end{equation}
$$

We now define the last relation $\mathcal{R}_{\mathrm {gnr-cond}}^{\mathfrak{S}^{\prime}}$ that specifies the valid i/o connection condition on two CF pairs (i.e. the relation on whether we can fold two CF pairs). This relation comes with an associated set $\mathcal{W}_{\mathrm {pvr-aux}}$ of all valid auxiliary witnesses in the forms of fixed-length vectors over $\mathbb{F}$ s.t. we can fold two CF pairs $\llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}$ and $\llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}}$ with auxiliary witness $\mathbf{w} \in \mathcal{W}_{\mathrm {pvr-aux}}$:

$$
\begin{equation}
\mathcal{R}_{\mathrm {gnr-cond}}^{\mathfrak{S}^{\prime}}=\left\{\begin{array}{l|l}
\left(\llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}, \llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}} ; \mathbf{w}\right) & \begin{array}{l}
\mathbf{w} \in \mathcal{W}_{\mathrm {pvr-aux}} \wedge \mathrm{rt}_{0}=\mathrm{lt}_{1} \\
\wedge \mathbf{A}^{\prime} \cdot \mathbf{c} \circ \mathbf{B}^{\prime} \cdot \mathbf{c}=\mathbf{C}^{\prime} \cdot \mathbf{c}
\end{array}
\end{array}\right\}
\end{equation}
$$

where $\mathbf{c}=\left(1\left\|\mathbf{o}_{0}\right\| \mathbf{i}_{1} \| \mathbf{w}\right)$ and $\mathbf{A}^{\prime}, \mathbf{B}^{\prime}$ and $\mathbf{C}^{\prime}$ are matrices in $\mathfrak{S}^{\prime}$. This GR-R1CS structure specifies conditions between consecutive computation steps $\llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}$ and $\llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}}$ by the *internal* constraints that enforce $\mathbf{o}_{0} = \mathbf{i}_{1}$, i.e. the output of the left computation steps must be the input of the right computation steps. Also folding these two pairs enforces the *external* constraint $\mathrm{rt}_{0}=\mathrm{lt}_{1}$, yielding $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$ representing steps from $\mathrm{lt}_{0}+1$ to $\mathrm{rt}_{1}$.

The last two relations $\mathcal{R}_{\mathrm {gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}$ and $\mathcal{R}_{\mathrm {gnr-cond}}^{\mathfrak{S}^{\prime}}$ together specify whether two CF pairs can be folded, i.e. they must be well-formed and satisfy the i/o connection condition.

## 2.3 A folding protocol for CF pairs

We first describe protocol $\mathit{\Pi}_{\mathrm{fold} \text{-gnr}}$ for folding two CF pairs $\llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}$ and $\llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}}$ into $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$. Then, in the next section, we employ this protocol as a building block to construct a generic CF scheme.

<div id="construction-fold-gnr"></div>
$\textbf{Construction 2: Protocol } \mathit{\Pi}_{\text{fold-gnr}}$

Given two CF pairs $\llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}$ and $\llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}}$ in the form of [#](#eq:structure-of-valid-CF-pair), the folding protocol $\mathit{\Pi}_{\text{fold-gnr}}$ proceeds as follows:\\\\
$\underline{\Pi_{\text{fold-gnr}}\left(\mathrm{pp}, \mathfrak{S}, \mathfrak{S}^{\prime}, \llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}, \llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}} ; \mathbf{w}\right) \rightarrow \llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}}$

1. Prover: calculate the garb vectors in the form of [#](./generic-relaxed-r1cs.html#eq:garb-vector-for-GR-R1CS)
  - $\mathbf{g} \leftarrow \operatorname{garb}\left(\mathfrak{S}, \llbracket \mathbb{x}_{0} \rrbracket_{\mathrm{tck}}, \llbracket \mathbb{x}_{1} \rrbracket_{\text{tck}}\right)$
  - $\mathbf{g}_{1} \leftarrow \operatorname{garb}\left(\mathfrak{S}^{\prime}, \llbracket x_{0}^{\star} \rrbracket_{\text{tck}^{\prime}}, \llbracket x_{1}^{\star} \rrbracket_{\text{tck}^{\prime}}\right)$
2. Both parties run the protocol $\mathit{\Pi}_{\text{com}}$ (defined in [#](./generic-relaxed-r1cs.html#eq:vector-commitment-protocol)) to commit to the below components:
  - $\llbracket \mathbf{g} \rrbracket_{\mathrm{cke}} \leftarrow \mathit{\Pi}_{\mathrm{com}}(\mathrm{cke} ; \mathbf{g})$
  - $\llbracket \mathbf{w} \rrbracket_{\mathrm{ck}_{3}^{\prime}} \leftarrow \mathit{\Pi}_{\mathrm{com}}\left(\mathrm{ck}_{3}^{\prime} ; \mathbf{w}\right)$
  - $\llbracket \mathbf{g}_{1} \rrbracket_{\mathrm{cke}^{\prime}} \leftarrow \mathit{\Pi}_{\mathrm{com}}\left(\mathrm{cke}^{\prime} ; \mathbf{g}_{1}\right)$

Then both parties form the GR-R1CS pair for i/o connection (follows the form in [#](./generic-relaxed-r1cs.html#eq:committed-instance-witness-pair)) which satisfies $\mathcal{R}_{\mathrm{GR-R1CS}}^{\mathfrak{S}^{\prime}}$ as follows:
  $$
\begin{aligned}
& \llbracket \mathbb{y} \rrbracket_{\mathrm{tck}}=\left(\mathbb{y}.u, \quad \mathbb{y}.\mathrm{pub}, \quad \llbracket \mathbb{y}.\mathbf{z}_{1} \rrbracket_{\mathrm{ck}_{1}^{\prime}}, \quad \llbracket \mathbb{y}.\mathbf{z}_{2} \rrbracket_{\mathrm{ck}_{2}^{\prime}}, \quad \llbracket \mathbb{y}.\mathbf{z}_{3} \rrbracket_{\mathrm{ck}_{3}^{\prime}}, \quad \llbracket \mathbb{y}.\mathbf{e} \rrbracket_{\mathrm{cke}^{\prime}}\right) \\
& :=\quad\left(1, \quad 1, \quad \llbracket \mathbf{o}_{0} \rrbracket_{\mathrm{ck}_{2}}, \quad \llbracket \mathbf{i}_{1} \rrbracket_{\mathrm{ck}_{1}}, \quad \llbracket \mathbf{w} \rrbracket_{\mathrm{ck}_{3}^{\prime}}, \quad \llbracket \mathbf{0}^{n^{\prime}} \rrbracket_{\mathrm{cke}^{\prime}}\right) .
\end{aligned}
$$

Notice that $\mathrm{ck}_{1}^{\prime}=\mathrm{ck}_{2}$ and $\mathrm{ck}_{2}^{\prime}=\mathrm{ck}_{1}$ as designed in [#](#eq:relation-between-first-two-ck-in-CF-fair) and the commitment $\llbracket \mathbf{0}^{n^{\prime}} \rrbracket_{\mathrm{cke}^{\prime}}$ can be determined by verifier alone.

3. Verifier: Abort if $\mathrm{rt}_{0} \neq \mathrm{lt}_{1}$. Otherwise, $\alpha_{1} \stackrel{\$}{\leftarrow} \mathbb{F}$ and send $\alpha_{1}$ to prover.

4. Both parties run step 4 in $\mathit{\Pi}_{\text{GR-R1CS}}$ (see [#](./generic-relaxed-r1cs.html#step-4-folding-GR-R1CS-pair)) to fold with the randomness $\alpha_{1}$:
  - step computation folding: $\llbracket \mathbb{x} \rrbracket_{\text{tck}} \leftarrow \mathit{\Pi}_{\text{GR-R1CS}}(\mathrm{tck} ; \mathfrak{S}; \llbracket x_{0} \rrbracket_{\text{tck}}, \llbracket x_{1} \rrbracket_{\text{tck}})$
  - i/o connection folding: $\llbracket \mathbb{y}^{\prime} \rrbracket_{\text{tck}} \leftarrow \mathit{\Pi}_{\text{GR-R1CS}}(\mathrm{tck}^{\prime} ; \mathfrak{S}^{\prime}; \llbracket x_{0}^{\star} \rrbracket_{\mathrm{tck}}, \llbracket x_{1}^{\star} \rrbracket_{\mathrm{tck}})$

5. Prover: Compute the garb vector for the *second* i/o connection folding
  $$\begin{aligned}
    \mathbf{g}_{2} \leftarrow \operatorname{garb}\left(\mathfrak{S}^{\prime}, \llbracket \mathbb{y}^{\prime} \rrbracket_{\mathrm{tck}^{\prime}}, \llbracket \mathbb{y} \rrbracket_{\mathrm{tck}}\right)
  \end{aligned}$$
6. Both parties run $\llbracket \mathbf{g}_{2} \rrbracket_{\text{cke}^{\prime}} \leftarrow \mathit{\Pi}_{\text{com}}(\mathrm{cke}^{\prime} ; \mathbf{g}_{2})$.
7. Verifier: $\alpha_{2} \stackrel{\$}{\leftarrow} \mathbb{F}$ and send $\alpha_{2}$ to prover.
8. Both parties run step 4 in $\mathit{\Pi}_{\mathrm{GR-R1CS}}$ to fold with the randomness $\alpha_{2}$:
  $$\begin{aligned}
    \llbracket \mathbb{x}^{\star} \rrbracket_{\mathrm{tck}^{\prime}} \leftarrow \mathit{\Pi}_{\mathrm{GR-R1CS}}(\mathrm{tck}^{\prime} ; \mathfrak{S}^{\prime}; \llbracket \mathbb{y}^{\prime} \rrbracket_{\mathrm{tck}^{\prime}}, \llbracket \mathbb{y} \rrbracket_{\mathrm{tck}^{\prime}})
  \end{aligned}$$
9. Finally, both parties compute the remaining components of the folded CF pair $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}}$:
  - i/o data components (as defined in [#](#eq:io-data-components)):
    $$
    \begin{array}{ll}
        \llbracket \mathbf{i} \rrbracket_{\mathrm{ck}_{1}}:=\llbracket \mathbf{i}_{0} \rrbracket_{\mathrm{ck}_{1}}, \llbracket \mathbf{o} \rrbracket_{\mathrm{ck}_{2}}:=\llbracket \mathbf{o}_{1} \rrbracket_{\mathrm{ck}_{2}}
      \end{array}
      $$
  - i/o connection components (as defined in [#](#eq:io-connection-components)):
    $$
    \begin{array}{ll}
      \llbracket \mathbf{s} \rrbracket_{\mathrm{ck}_{5}}:=\llbracket \mathbf{s}_{0} \rrbracket_{\mathrm{ck}_{5}}+\llbracket \mathbf{s}_{1} \rrbracket_{\mathrm{ck}_{5}}, \llbracket \mathbf{a} \rrbracket_{\mathrm{ck}_{5}}:=\llbracket \mathbf{a}_{0} \rrbracket_{\mathrm{ck}_{5}}+\alpha_{1} \cdot \llbracket \mathbf{a}_{1} \rrbracket_{\mathrm{ck}_{5}}+\alpha_{1}^{2} \cdot\left(\llbracket \mathbf{s}_{0} \rrbracket_{\mathrm{ck}_{5}}-\llbracket \mathbf{s}_{1} \rrbracket_{\mathrm{ck}_{5}}\right)
    \end{array}
    $$

At this point, both parties achieve the folded CF pair
  $$\begin{aligned}
    \llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} = (  \mathrm{lt}_0, \mathrm{rt}_1, \llbracket \mathbb{x} \rrbracket_{\mathrm{tck}}, \llbracket \mathbf{i} \rrbracket_{\mathrm{ck}_1}, \llbracket \mathbf{o} \rrbracket_{\mathrm{ck}_2}, \llbracket \mathbb{x}^{\star} \rrbracket_{\mathrm{tck}^{\prime}}, \llbracket \mathbf{s} \rrbracket_{\mathrm{ck}_5}, \llbracket \mathbf{a} \rrbracket_{\mathrm{ck}_5})
  \end{aligned}$$

## 2.4 A generic construction of CF Scheme

The construction of CF scheme $\mathcal{C} \mathcal{F}_{\text{gnr}}$ can be constructed from the CF protocol $\mathit{\Pi}_{\text{fold-gnr}}$ straightforwardly. Let $\mathfrak{S}=(\mathbf{A}, \mathbf{B}, \mathbf{C}), \mathfrak{S}^{\prime}=\left(\mathbf{A}^{\prime}, \mathbf{B}^{\prime}, \mathbf{C}^{\prime}\right)$ be described as above. Let $\mathcal{C}$ be a homomorphic commitment scheme.

In this part, we construct the generic CF scheme $\mathcal{C} \mathcal{F}_{\text{gnr}}\left[\mathcal{R}_{\text{gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}, \mathcal{R}_{\text{gnr-cond}}^{\mathfrak{S}^{\prime}}\right]=$ (CF.Setup, CF.Fold, CF.Prove) for relation $\mathcal{R}_{\text{gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}$ with respect to condition relation $\mathcal{R}_{\text{gnr-cond}}^{\mathfrak{S}^{\prime}}$, based on the protocol $\mathit{\Pi}_{\text{fold-gnr}}$ above.

<div id="cf-gnr"></div>
$\textbf{Construction 3: CF Scheme } \mathcal{C} \mathcal{F}_{\text{gnr}}$

$\textbf{CF.Setup}$ $\left(1^{\lambda}\right) \rightarrow \mathrm{pp}$:
- Sample $\mathrm{ck}_{1}, \ldots, \mathrm{ck}_{5}, \mathrm{cke}, \mathrm{ck}_{3}^{\prime}, \mathrm{cke}^{\prime}$ from C.Setup. Set $\mathrm{ck}_{1}^{\prime}:=\mathrm{ck}_{2}, \mathrm{ck}_{2}^{\prime}:=\mathrm{ck}_{1}$.
- Form tuples tck $:=\left(\mathrm{ck}_{1}, \ldots, \mathrm{ck}_{5}, \mathrm{cke}\right)$ and $\mathrm{tck}:=\left(\mathrm{ck}_{1}^{\prime}, \mathrm{ck}_{2}^{\prime}, \mathrm{ck}_{3}^{\prime}, \mathrm{cke}^{\prime}\right)$.
- Return $\mathrm{pp}:=(\mathrm{tck}, \mathrm{tck})$.

$\textbf{CF.Fold}$ (pp, $\left.\llbracket \mathbb{z}_{0} \rrbracket_{\text{tck}}, \llbracket \mathbb{z}_{1} \rrbracket_{\text{tck}} ; \mathbf{w}\right) \rightarrow \llbracket \mathbb{z} \rrbracket_{\text{tck}}:$ Both parties run $\mathit{\Pi}_{\text{fold-gnr}}$ (see [#](#construction-fold-gnr)):
$$\begin{aligned}
  \llbracket \mathbb{z} \rrbracket_{\text{tck}} \leftarrow \mathit{\Pi}_{\text{fold-gnr}}\left(\mathrm{pp}, \mathfrak{S}, \mathfrak{S}^{\prime}, \llbracket \mathbb{z}_{0} \rrbracket_{\mathrm{pp}}, \llbracket \mathbb{z}_{1} \rrbracket_{\mathrm{pp}} ; \mathbf{w}\right).
\end{aligned}$$

$\textbf{CF.Prove}$ $(p p, \llbracket \mathbb{z} \rrbracket) \rightarrow\{0,1\}$ : Prover runs a (HV)ZKAoK/PoK protocol for $\llbracket \mathbb{z} \rrbracket_{\mathrm{pp}} \in \mathcal{R}_{\text{gnr-inst}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}$

The scheme satisfies perfect completeness, HVZK and knowledge soundness with soundness error $\mathcal{O}\left(N /|\mathbb{F}|+\operatorname{serr}_{\mathrm{prf}}(\mathrm{pp})+\operatorname{negl}(\lambda)\right)$ where $\operatorname{serr}_{\mathrm{prf}}(\mathrm{pp})$ is the soundness error of $\textbf{CF.Prove}$. The full security analysis is presented in [{{#cite TPN24}}, Appdx. B.1].

Here we briefly discuss the efficiency of the scheme. Let $N$ be the number of steps in the IVC program, $\mathrm{c}(k)$ be the size of commitments to messages in $\mathbb{F}^{k}$, $\mathrm{p}$, $\mathrm{tp}$ and $\mathrm{tv}$ be the communication cost, prover time and verifier time of CF.Prove respectively.

- Linear sequential folding:
  - Communication cost: $\mathcal{O}\left(N \cdot\left(\mathrm{c}(n)+\mathrm{c}\left(m_{3}^{\prime}\right)+\mathrm{c}\left(n^{\prime}\right)\right)+\mathrm{p}\right)$
  - Prover time: $\mathcal{O}(\mathrm{tp})$
  - Verifier time: $\mathcal{O}(\mathrm{tv})$

- Parallel folding following a binary tree of depth $\mathcal{O}(\log N)$:
  - Communication cost: $\mathcal{O}\left(\left(\mathrm{c}(n)+\mathrm{c}\left(m_{3}^{\prime}\right)+\mathrm{c}\left(n^{\prime}\right)\right)+\mathrm{p}\right)$
  - Prover time: $\mathcal{O}(|W| \log N+\mathrm{tp})$, in which $\mathcal{O}(|W| \log N)$ is the cost for prover to execute CF.Fold, where $|W|$ is the size of witness in an instance-witness pair. Here we assume that witness size is at least linear in instance size since instances in the CF scheme are a collection of commitments
  - Verifier time: $\mathcal{O}(|W| \log N+\mathrm{tv})$
