# 3. RAMenPaSTA: A Generic pvRAM Construction from the Conditional Folding Scheme

Having established the conditional folding scheme $\mathcal{CF}_{\text{gnr}}$ in the previous section, we now present $\mathrm{RAMenPaSTA}$ - a generic construction of parallelizable verifiable RAM (pvRAM) program that leverages this scheme. We first describe the design of constraints for IVC computation of a RAM program, then we adapt the CF scheme to this pvRAM setting. Finally, we present a generic construction of pvRAM.

## 3.1 Constraints for IVC Computation of RAM Programs in the CF scheme

### 3.1.1 The Model of RAM Program

Recall that a pvRAM is a parallelizable proof/argument for RAM programs. We use the model of RAM program from {{#cite FKLO0W21}} that contains:
- an instruction set $\mathcal{F}$ containing $T$ instructions denoted by $\mathcal{F}=\left\{F_j^{\prime}\right\}_{j \in[T]}$
- a register containing $c_{\text{reg}}$ addresses for some small constant $c_{\text{reg}}$
- a memory mem $=\left(\text{mem }_i\right)_{i \in[M]}$ of $M$ addresses (cells)
- a program counter for determining the next atomic instruction, among $\mathcal{F}$

The computation in each step $i$ of a $N$-step RAM program $\Pi$, which executes an instruction $F_i$ from $\mathcal{F}$, can be described as follows:
$$
\begin{equation}
  (\mathrm{pc}_i, \mathrm{reg}_i, \ell_i, \mathrm{val}_i, \mathrm{mop}_i) = \Pi(\mathrm{pc}_{i-1}, \mathrm{reg}_{i-1}, \mathrm{val}_{i-1}), \forall i \in [1..N]
\end{equation}
$$
where:
- $\mathrm{pc}_i$ is the program counter for step $i$, which specifies the instruction to be executed in step $i + 1$. Here we denote $\overline{\mathrm{pc}}_i = \mathrm{pc}_{i-1}$ for brevity.
- The instruction $F_i$ of step $i$ is determined by the program counter of the previous step: $F_i := F_{\overline{\mathrm{pc}}_{i}}^{\prime}$. This instruction interacts (READ/WRITE) with the register and at most one memory location in the memory.
- $\mathrm{reg}_i$ is the state of the register for step $i$
- $\ell_i$ is the memory address accessed in step $i$
- $\mathrm{val}_i$ is the value read from/written to memory in step $i$
- $\mathrm{mop}_i$ is the memory access operation (READ/WRITE) in step $i$

Initially (before step 1), the inputs are $(\mathrm{pc}_0, \mathrm{reg}_0, \mathrm{val}_0)$: where
- $\mathrm{pc}_0 = 1$
- $\mathrm{reg}_0$ is set to some initial input to RAM program
- $\mathrm{val}_0$ is some initial dummy value

The final output of the program is written to $\mathrm{reg}_N$.

This RAM program model aligns well with the CF scheme $\mathcal{CF}_{\text{gnr}}$ for IVC computation discussed previously, as each individual step in the CF scheme can correspond to a single step in the RAM program.

### 3.1.2 Constructing CF pairs for RAM program

To construct a (probabilistic) proof for an $N$-step RAM program $\Pi$ ($N \in \mathbb{Z}_{+}$), we need to construct the following constraints:
- correct execution of each instruction
- correctly selecting instruction by program counter
- memory consistency (any read operation in the memory must be consistent with the most recent write operation to the same memory location)

#### 3.1.2.1 Correctness of Instruction Execution

First, to ensure the correctness of the instructions, we need a way to represent the instruction set $\mathcal{F}=\left\{F_{j}^{\prime}\right\}_{j \in[T]}, T \in \mathbb{Z}_{+}$ in the arithmetized format inside the SC-R1CS instances. An obvious choice is to represent the instructions in the native R1CS form. However, the authors chose to use the PLONK framework.

The intuition is that while R1CS has been widely used, it comes with certain drawbacks. Notably, the matrices in R1CS typically require quadratic size relative to the number of circuit gates. Although some optimizations exist for R1CS {{#cite S20}} {{#cite CHMMVW20}} {{#cite DXNT24}} based on sumcheck arguments {{#cite LFKN92}} {{#cite BCRSSVW19}}, they may not be universally applicable to other assumptions and proof techniques used in the paper, e.g. MPC-in-the-head, dual-mode NIWIs, which don't rely solely on sumcheck. In contrast, PLONK {{#cite GWC19}} offers significant advantages. Its structure allows for a more efficient and flexible representation of computational logic, which can handle complex circuits with less overhead.

The PLONK structure has two main components:
- selectors: effectively capture correct gate computations
- permutations: capture wire connections


To universally ensure the correctness of an instruction in the form of a PLONK structure inside an GR-R1CS structure—which is the main structure for the CF scheme that we will use—we must build a PLONK verifier circuit. It is constructed as follows:
- Selector consistency: the circuit checks the consistency between the corresponding selectors and secrets of the arithmetized form of $F^{\prime}$, which is crucial for verifying the integrity of the computation
- Permutation argument for copy constraint consistency: we use two additional inputs, $\gamma$ and $\delta$, to guarantee copy constraints, and we utilize a grand product argument incorporating $\gamma$ and $\delta$ to universally ensure that the permutation argument is satisfied


We now encode each instruction $F^{\prime}_j \in \mathcal{F}$ as a PLONK structure $\mathrm{plkst}_j^{\prime}$ in the form of a fixed-length vector over $\mathbb{F}$ containing the selectors and permutations.

#### 3.1.2.2 Correctness of Instruction Selection

To ensure correct instruction selection, we must verify that for each step $i \in [N]$, the chosen instruction $F_i$ corresponds to the previous step's program counter $\overline{\mathrm{pc}}_{i}$. Specifically, we need to confirm that the PLONK structure $\mathrm{plkst}_i = \mathrm{plkst}^{\prime}_{\overline{\mathrm{pc}}_{i}}$ (representing $F_i$) is correctly picked from the set of all possible PLONK structures $\{\mathrm{plkst}_j^\prime\}_{j \in [T]}$ based on $\overline{\mathrm{pc}}_{i}$. This requirement can be formalized as:

<div id="eq:instruction_selection_validity_plonk_structure_subset_relationship"></div>
$$
\begin{equation}
\{(\overline{\mathrm{pc}}_i \| \mathrm{plkst}_i)\}_{i \in [N]} \subset \{(j \| \mathrm{plkst}_j^\prime)\}_{j \in [T]}
\end{equation}
$$

We can apply Lemma 7 of {{#cite TPN24}} which is an adapted version of the tuple lookup lemma from {{#cite H22}} (detailed in Appendix A.4 of {{#cite TPN24}}) to verify this subset relationship.

**Tuple Lookup Lemma - Lemma 7 of** {{#cite TPN24}}
<div id="lemma:tuple_lookup_lemma"></div>

Given sequences $\mathbf{a}=\left(\mathbf{a}_i\right)_{i=1}^n \in\left(\mathbb{F}^s\right)^n$ and $\mathbf{b}=\left(\mathbf{b}_j\right)_{j=1}^t \in\left(\mathbb{F}^s\right)^t$ where $s, n$ and $t$ are positive integers. For a vector $\mathbf{v} \in \mathbb{F}^s$, define $f_{\mathbf{v}}(Y)=$ $\left\langle\mathbf{v},\left(Y^k\right)_{k=0}^{s-1}\right\rangle$. Assume that

$$
\sum_{i=1}^n \frac{1}{X+f_{\mathbf{a}_i}(Y)} \neq \sum_{j=1}^t \frac{\mathrm{mul}_j}{X+f_{\mathbf{b}_j}(Y)}
$$

for some $\mathrm{mul}_1, \ldots$, mul $_t \in \mathbb{F}$. Then,

$$
\operatorname{Pr}\left[f(\chi, \psi)=0 \mid \psi \stackrel{\S}{\leftarrow} \mathbb{F} \wedge \chi \stackrel{s}{\leftarrow} \mathbb{F} \backslash\left\{-f_{\mathbf{b}_i}(\psi)\right\}_{i=1}^t\right] \leq \frac{(s-1)(n+t-1)}{|\mathbb{F}|-t}
$$

where

$$
f(X, Y)=\sum_{i=1}^n \frac{1}{X+f_{\mathbf{a}_i}(Y)}-\sum_{j=1}^t \frac{\mathrm{mul}_j}{X+f_{\mathbf{b}_j}(Y)}
$$

Following the lemma, by sampling $(\chi, \psi)$ uniformly from $\mathbb{F} \times \mathbb{F}$ and setting $(X, Y):=(\chi, \psi)$, we see that, the subset relationship [#](#eq:instruction_selection_validity_plonk_structure_subset_relationship) holds iff:
<div id="eq:instruction_selection_validity_check_via_grand_sum"></div>
$$
\begin{equation}
  \sum_{i=1}^{N} \frac{1}{X+\left\langle\left({\overline{\mathrm{pc}}}_{i} \| \mathrm{plkst}_{i}\right),\left(Y^{k}\right)_{k=\left[0, n_{\mathrm{plk}}\right]}\right\rangle}=\sum_{j=1}^{T} \frac{\mathrm{mul}_{j}}{X+\left\langle\left(j \| \mathrm{plkst}_{j}^{\prime}\right),\left(Y^{k}\right)_{k \in\left[0, n_{\mathrm{plk}}\right]}\right\rangle}
\end{equation}
$$

except for a soundness error at most $\mathrm{err}_{\text{lookup}}\left(\mathbb{F}, n_{\text{plk}}+1, N, T\right)=\mathcal{O}\left(n_{\text{plk}}(N+\right.$ $T) /|\mathbb{F}|$, where
- $n_{\mathrm{plk}}$ is the length of the vector representation of the PLONK structure of the instructions
- each $\mathrm{mul}_{j}$ is determined by prover by counting multiplicity (number of appearances) of $\left(j \| \mathrm{plkst}_{j}^{\prime}\right)$ in $\{(\overline{\mathrm{pc}}_i \| \mathrm{plkst}_i)\}_{i \in [N]}$

We can construct an argument to check this grand sum equality. However, in [#](#eq:instruction_selection_validity_check_via_grand_sum), we may face division by zero for some bad choice of $(\chi, \psi)$. The solution is quite straightforward: let the verifier sample $\psi$ first, then it will sample $\chi$ from the set of numbers what won't yield the zero denominator.

Specifically, the verifier samples $\psi \stackrel{\$}{\leftarrow} \mathbb{F}$, sets $\boldsymbol{\psi} = (\psi^k)_{k \in [0, n_{\mathrm{plk}}]}$ and:
$$
\begin{equation}
  \mathrm{plkcp}_{i} := \left\langle\left({\overline{\mathrm{pc}}}_{i} \| \mathrm{plkst}_{i}\right), \boldsymbol{\psi}\right\rangle \forall i \in[N] , \quad \mathrm{plkcp}^{\prime}_{j} := \left\langle\left(j \| \mathrm{plkst}_{j}^{\prime}\right), \boldsymbol{\psi}\right\rangle \forall j \in[T]
\end{equation}
$$

At this point, [#](#eq:instruction_selection_validity_check_via_grand_sum) can be written as $\sum_{i=1}^{N} \frac{1}{X+\mathrm{plkcp}_{i}}=\sum_{j=1}^{T} \frac{\mathrm{mul}_{j}}{X+\mathrm{plkcp}_{j}^{\prime}}$, equivalent to proving $\left\{- \mathrm{plkcp}_{i}\right\}_{i \in[N]} \subseteq\left\{- \mathrm{plkcp}_{j}^{\prime}\right\}_{j \in[T]}$.

Verifier now can sample $\chi \stackrel{\$}{\leftarrow} \mathbb{F}$ \ $\left(\left\{-\mathrm{plkcp}_{i}\right\}_{i \in[N]} \cup\left\{- \mathrm{plkcp}_{j}^{\prime}\right\}_{j \in[T]}\right)$ to avoid division by zero. Since during the execution of the protocol, the verifier only knows the available instruction set $\left\{-\mathrm{plkcp}_{j}^{\prime}\right\}_{j \in[T]}$, it suffices for the verifier to sample $\chi \stackrel{s}{\leftarrow} \mathbb{F}$ \ $\left\{- \mathrm { plkcp }_j^{\prime}\right\}_{j \in[T]}$. If the prover uses only valid instructions, there won't be any division by zero.

Again we introduce new notations to make this equality more clean:
<div id="eq:plkiv_computation"></div>
$$
\begin{equation}
\operatorname{plkiv}_{i}=\left(\chi+\operatorname{plkcp}_{i}\right)^{-1} \forall i \in[N], \quad \operatorname{plkiv}_{j}^{\prime}=\operatorname{mul}_{j} \cdot\left(\chi+\operatorname{plkcp}_{j}^{\prime}\right)^{-1} \forall j \in[T]
\end{equation}
$$

Now testing [#](#eq:instruction_selection_validity_plonk_structure_subset_relationship) can be deduced to checking that

<div id="eq:plkiv_consistency"></div>
$$
\begin{equation}
\sum_{i=1}^{N} \operatorname{plkiv}_{i}=\sum_{j=1}^{T} \operatorname{plkiv}_{j}^{\prime}
\end{equation}
$$

with error probability at most $\operatorname{err}_{\text{lookup}}\left(\mathbb{F}, n_{\text{plk}}+1, N, T\right)$.

Finally, we have to make sure the inverses are correct by checking that $\mathrm{plkiv}_{i} \cdot (\chi+\mathrm{plkcp}_{i})=1$ when specify constraints.

<!-- % TODO: comeback to make it clear between the notations of X and \chi -->

#### 3.1.2.3 Consistency of Memory Accesses

This property can be described briefly as: each read operation at a given address in the RAM must be consistent with the most recent write operation at the same address.

The memory consistency checks are derived from {{#cite FKLO0W21}}, which is a form of *sorting technique*. The idea of sorting technique is to sort the memory accesses by a tuple of (memory location, timestamp) in non-decreasing order. This way, the operations on the same memory location are adjacent in the sorted list and we can do consistency checks on all the accessed memory locations in only a linear scan.

Following the notations from {{#cite FKLO0W21}}, the $i$-th memory access (performed by the instruction at step $i$) is represented by a constant-length vector $\mathrm{macs}_i = (\ell_i, \mathrm{time}_i, \mathrm{val}_i, \mathrm{mop}_i) \in \mathbb{F}^4$, where $\ell_i$ is the memory address, $\mathrm{time}_i$ is the timestamp, $\mathrm{val}_i$ is the value, and $\mathrm{mop}_i$ is the memory operation type. The sorted list is denoted as $(\mathrm{macs}_i^{\prime} = (\ell_i^{\prime}, \mathrm{time}_i^{\prime}, \mathrm{val}_i^{\prime}, \mathrm{mop}_i^{\prime}))_{i \in [N]}$, which is the supporting list for the consistency check. Then the consistency of memory accesses can be enforced by the following constraints:
- <div id="eq:memory_consistency_check_valid_original_list"></div> Valid original list of memory access: $1 \leq \ell_i \leq M \wedge \operatorname{mop}_i \in\{0,1\} \forall i \in[N] \wedge \mathrm{time}_{i-1}<\mathrm{time}_i \forall i \in [2, N]$
- The supporting list $(\text{macs}_i^{\prime})_{i \in[N]}$ must be a permutation of the original list: $(\text{macs}_i)_{i \in[N]}$
- <div id="eq:memory_consistency_check"></div> The supporting list must be sorted and the memory accesses are consistent: $\forall i \in[2, N]: (\ell_{i-1}^{\prime}<\ell_i^{\prime}) \vee((\ell_{i-1}^{\prime}=\ell_i^{\prime}) \wedge(..$ time $_{i-1}^{\prime}<$ time $.._i^{\prime}))$, $(.$ macs $_{i-1}=$ 0) $\vee(i-1>1),(\ell_{i-1}^{\prime}=\ell_i^{\prime}) \vee(\mathrm{mop}_i^{\prime}=0)$ and $(\ell_{i-1}^{\prime} \neq \ell_i^{\prime}) \vee(\mathrm{val}_{i-1}^{\prime}=\mathrm{val}_i^{\prime}) \vee(\mathrm{mop}_i^{\prime}=0)$

The tricky part is the second permutation check.

We again apply the tuple lookup lemma [#](#lemma:tuple_lookup_lemma) to show that $\left(\operatorname{macs}_{i}\right)_{i \in[N]}$ is a permutation of $\left(\operatorname{macs}_{i}^{\prime}\right)_{i \in[N]}$. This holds iff:

$$
\begin{equation}
\sum_{i=1}^{N} \frac{1}{X+\left\langle\operatorname{macs}_{i},\left(Y^{k}\right)_{k=0}^{3}\right\rangle}=\sum_{i=1}^{N} \frac{1}{X+\left\langle\operatorname{macs}_{i}^{\prime},\left(Y^{k}\right)_{k=0}^{3}\right\rangle}
\end{equation}
$$
except for a soundness error at most $\mathcal{O}(N/|\mathbb{F}|)$ when sampling $(X, Y):=(\tau, \omega)$ uniformly from $\mathbb{F} \times \mathbb{F}$.

We also apply the same sampling strategy as in the instruction selection validity check to avoid division by zero. However, this time both lists $\left(\operatorname{macs}_{i}\right)_{i \in[N]}$ and $\left(\operatorname{macs}_{i}^{\prime}\right)_{i \in[N]}$ are unknown to the verifier. Hence, the verifier cannot sample challenges avoiding division by zero. To address this, let $\omega \stackrel{\$}{\leftarrow} \mathbb{F}$ be determined first. Then, set

$$
\begin{equation}
\mathrm{mcp}_{i}:=\left\langle\operatorname{macs}_{i}, \boldsymbol{\omega}\right\rangle, \mathrm{mcp}_{i}^{\prime}:=\left\langle\operatorname{macs}_{i}^{\prime}, \boldsymbol{\omega}\right\rangle \quad \forall i \in[N]
\end{equation}
$$

where $\boldsymbol{\omega} = (\omega^k)_{k \in [0, 3]}$.

Then if we sample $\tau \stackrel{\$}{\leftarrow} \mathbb{F}$, with probability at most $2 N /|\mathbb{F}|,-\tau \in\left\{\mathrm{mcp}_{i}\right\}_{i \in[N]} \cup\left\{\mathrm{mcp}_{i}^{\prime}\right\}_{i \in[N]}$ incurring division by zero. We call $\tau$ in this case to be \textit{bad $\tau$}. This limits the prover's ability to prove in case of bad $\tau$ althought the prover is following the protocol correctly, hence compromising the HVZK property.

To avoid this, we need to devise a way to allow prover to continue proving even when bad $\tau$ occurs. The chosen approach is to use a default backup value for the individual fraction in case zero-denominator occurs.

The prover samples $\tau \stackrel{\$}{\leftarrow} \mathbb{F}^{\star}$ and then sets
$$
\begin{equation}
\operatorname{miv}_{i}:=\begin{cases}
\text{iv} & \text{if }\tau+\operatorname{mcp}_{i}=0\\
\left(\tau+\mathrm{mcp}_{i}\right)^{-1} & \text{otherwise}
\end{cases}
\end{equation}
$$

Now we can do a probabilistic test to check that $(\operatorname{macs}_{i})_{i \in[N]}$ is a permutation of $(\operatorname{macs}_{i}^{\prime})_{i \in[N]}$ without making prover stop in the middle when bad $\tau$ occurs.

<div id="eq:miv_consistency"></div>
$$
\begin{equation}
\sum_{i=1}^{N} \operatorname{miv}_{i}=\sum_{i=1}^{N} \operatorname{miv}_{i}^{\prime}
\end{equation}
$$

This helps protect the HVZK property of the protocol. Moreover, since bad $\tau$ occurs with negligible probability, namely, $\mathcal{O}(N /|\mathbb{F}|)$, bad $\tau$ only affects to soundness error of the protocol while preserving the other properties.

As usual, we still have to explicitly check whether the inverses are constructed correctly

<div id="eq:miv_check"></div>
$$
\begin{equation}
  \operatorname{miv}_i=\left(\tau+\mathrm{mcp}_i\right)^{-1} \Longleftrightarrow \operatorname{miv}_i \neq 0 \wedge\left(\tau+\mathrm{mcp}_i\right) \cdot \operatorname{miv}_i \in\{0,1\}
\end{equation}
$$

This is similar to how we check the correctness of $\mathrm{plkiv}_i$ in [plkiv computation](#eq:plkiv_computation).

#### 3.1.2.4 Construction of a CF pair for pvRAM

Now we are ready to build the CF pair for an IVC computation of the pvRAM program that can be used in $\mathcal{CF}_{\mathrm{gnr}}$. Let's denote $\mathbf{gchal} = (\gamma, \delta, \chi, \psi, \tau, \omega)$ to contain global challenges.

We recall the components at each step $i \in[N]$, including:
$$
\overline{\mathrm{pc}}_{i}, \overline{\mathrm{reg}}_{i}, \overline{\operatorname{macs}}_{i}, \mathrm{aux}_{i}, \mathrm{plkst}_{i}, \mathrm{pc}_{i}, \operatorname{reg}_{i}, \operatorname{macs}_{i}, \operatorname{macs}_{i}^{\prime}, \operatorname{plkiv}_{i}, \operatorname{miv}_{i}, \operatorname{miv}_{i}^{\prime}
$$
where at each step $i \in[N]$, the following must hold:
- correct computation of $F_i$ involving $\overline{\mathrm{reg}}_i, \overline{\mathrm{macs}}_i, \mathrm{aux}_i, \mathrm{plkst}_i, \mathrm{pc}_i, \mathrm{reg}_i, \mathrm{macs}_i, \gamma, \delta ;$
- memory consistency constraints involving $\mathrm{macs}_i$ hold, e.g., access index is in allowed range $[M]$, the consistency between two consecutive memory accesses $\mathrm{macs}_i^{\prime}$ and $\mathrm{macs}_{i-1}^{\prime}$ in the sorted list 
- the correct computation of $\mathrm{plkiv}_i$ from $\left(\overline{\mathrm{pc}}_i, \mathrm{plkst}_i\right), \chi$ and $\psi$, as in [plkiv computation](#eq:plkiv_computation); and
- the correct computation of $\mathrm{miv}_i$ and $\mathrm{miv}_i^{\prime}$ from $\mathrm{macs}_i, \mathrm{macs}_i^{\prime}, \tau$ and $\omega$, i.e., satisfying the RHS of [#](#eq:miv_check).

Moreover, the global constraints for all computation steps are:
- the correct use of $\mathrm{plkst}_i$ with respect to $\overline{\mathrm{pc}}_i=\mathrm{pc}_{i-1}$ by checking the lookup $\left\{\left(\overline{\mathrm{pc}}_i \| \mathrm{plkst}_i\right)\right\}_{i \in[N]} \subseteq$
    $\left\{\left(j \| \text{plkst }_j^{\prime}\right)\right\}_{j \in[T]}$ as in [plkiv consistency](#eq:plkiv_consistency)
- $\left(\operatorname{macs}_i^{\prime}\right)_{i \in[N]}$ is a permutation of $\left(\text{macs }_i\right)_{i \in[N]}$ as in [miv consistency](#eq:miv_consistency)

We can construct a CF pair $\mathbb{z}_{(i-1)i}$ for an individual computation step $i \in[N]$ following the syntax from [#](./conditional-folding-scheme.html#def:cf_instance_witness_pair) as follows (here the subscript $(i-1)i$ is used to indicate that this CF pair is for the step $i$ which use the output of the step $i-1$ as input):
$$
\mathbb{z}_{(i-1)i}=(\mathrm{lt}_{(i-1)i}, \mathrm{rt}_{(i-1)i}, \mathbb{x}_{(i-1)i}, \mathbf{i}_{(i-1)i}, \mathbf{o}_{(i-1)i}, \mathbb{x}_{(i-1)i}^{\star}, \mathbf{s}_{(i-1)i}, \mathbf{a}_{(i-1)i})
$$ where the components are defined as follows:

$$
\begin{aligned}
\mathrm{lt}_{(i-1)i} & :=i-1, \mathrm{rt}_{(i-1)i}:=i,\\
\mathbf{i}_{(i-1)i} & :=\mathbb{x}_{(i-1)i}.\mathbf{z}_1, \mathbf{o}_{(i-1)i}:=\mathbb{x}_{(i-1)i}.\mathbf{z}_2, \\
\mathbf{s}_{(i-1)i} & :=\mathbb{x}_{(i-1)i}.\mathbf{z}_5, \mathbf{a}_{(i-1)i}:=\mathbf{0}^{3}, \\
\mathbb{x}_{(i-1)i} & :=(\mathbb{x}_{(i-1)i}.u, \mathbb{x}_{(i-1)i}.\mathrm{pub}, \mathbb{x}_{(i-1)i}.\mathbf{z}_{1}, \ldots, \mathbb{x}_{(i-1)i}.\mathbf{z}_{5}, \mathbb{x}_{(i-1)i}.\mathbf{e}) \\
& :=(1,1, \mathbb{x}_{(i-1)i}.\mathbf{z}_{1}, \ldots, \mathbb{x}_{(i-1)i}.\mathbf{z}_{5}, \mathbf{0}^{n}) \\
\mathbb{x}_{(i-1)i}^{\star} & :=(\mathbb{x}^{\star}_{(i-1)i}.u, \mathbb{x}^{\star}_{(i-1)i}.\mathrm{pub}, \mathbb{x}^{\star}_{(i-1)i}.\mathbf{z}_{1}, \ldots, \mathbb{x}^{\star}_{(i-1)i}.\mathbf{z}_{3}, \mathbb{x}^{\star}_{(i-1)i}.\mathbf{e})
\end{aligned}
$$

The components of $\mathbb{x}_{(i-1)i}$ are structured as follows:
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{1}=(\overline{\mathrm{pc}}_{i}\|\overline{\mathrm{reg}}_{i}\|{\overline{\mathrm{macs}}}_{i} \| \mathrm{macs}_{i}^{\star})$: contains the input of the step (note that $\mathrm{macs}_{i}^{\star}$ is supposed to be the same as $\mathrm{macs}_{i}^{\prime}$, which will be enforced by the R1CS structure)
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{2}=(\mathrm{pc}_{i}\|\mathrm{reg}_{i}\| \mathrm{macs}_{i} \| \mathrm{macs}_{i}^{\prime})$: contains the output of the step
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{3}=\mathrm{plkst}_{i}$: contains the PLONK structure for the instruction at step $i$
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{4}=\mathrm{aux}_{i}$: contains the auxiliary witness for the PLONK structure of the instruction computation (with input $\mathbb{x}_{(i-1)i}.\mathbf{z}_{1}$, output $\mathbb{x}_{(i-1)i}.\mathbf{z}_{2}$ and computation $\mathbb{x}_{(i-1)i}.\mathbf{z}_{3}$)
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{5}=(\mathrm{plkiv}_{i}\|\mathrm{miv}_{i}\| \mathrm{miv}_{i}^{\prime})$: contains the private values which supports tuple lookup and permutation testings

Note that we need additional challenges $\chi, \psi, \tau, \omega$ in $\mathrm{gchal}$ determined in advance s.t. relationship between $\mathbb{x}_{(i-1)i}.\mathbf{z}_{5}$ and $\mathbb{x}_{(i-1)i}.\mathbf{z}_{1}, \ldots, \mathbb{x}_{(i-1)i}.\mathbf{z}_{4}$ is captured, since $\mathbb{x}_{(i-1)i}.\mathbf{z}_{5}$ is dependent on these random challenges.

The GR-R1CS structure $\mathfrak{S} = (\mathbf{A}, \mathbf{B}, \mathbf{C})$ of $\mathbb{x}_{(i-1)i}$ encodes the following constraints:

- $\mathrm{macs}_{\mathrm{i}}^*=\text{macs}_i^{\prime}$
- $(\mathrm{pc}_i, \mathrm{reg}_i, \mathrm{macs}_i)=F_{\overline{\mathrm{pc}}_i}(\overline{\mathrm{reg}}_i,{\overline{\mathrm{macs}_i}}, \mathrm{aux}_i)$
- $\overline{\mathrm{macs}}_i$ and $\mathrm{macs}_i$ are related through the memory consistency constraints in [Memory consistency check](#eq:memory_consistency_check)
- $\mathrm{macs}_i$ have valid value components in [Memory consistency check valid original list](#eq:memory_consistency_check_valid_original_list)
- $\mathrm{plkst}_i$ is the PLONK structure of $F_{\overline{p c}_i}$ w.r.t randomness $\gamma, \delta$
- $\mathrm{plkiv}_i(\chi+\text{plkcp}_i)=1 \text{ where } \mathrm{plkcp}_i=\langle(\overline{\mathrm{pc}}_i \| \text{plkst}_i), \boldsymbol{\psi}\rangle$
- $\mathrm{miv}_i(\tau+\text{mcp}_i) \in\{0,1\}{\text{ where } \mathrm{mcp}_i}=\langle\mathrm{macs}_i, \boldsymbol{\omega}\rangle$
- $\mathrm{miv}_i^{\prime}(\tau+\mathrm{mcp}_i^{\prime}) \in\{0,1\} \text{ where } \mathrm{mcp}_i^{\prime}=\langle\text{macs}_i^{\prime}, \boldsymbol{\omega}\rangle$

The components of $\mathbb{x}^{\star}_{(i-1)i}$ are structured as follows:
- $\mathbb{x}^{\star}_{(i-1)i}.\mathbf{z}_{1}=\mathbb{x}_{(i-1)i}.\mathbf{z}_{2} = (\mathrm{pc}_{i}\|\mathrm{reg}_{i}\| \mathrm{macs}_{i} \| \mathrm{macs}_{i}^{\prime})$: contains the output of the step
- $\mathbb{x}^{\star}_{(i-1)i}.\mathbf{z}_{2}=\mathbb{x}_{(i-1)i}.\mathbf{z}_{1}= (\overline{\mathrm{pc}}_{i}\|\overline{\mathrm{reg}}_{i}\|{\overline{\mathrm{macs}}}_{i} \| \mathrm{macs}_{i}^{\star})$: contains the input of the step
- $\mathbb{x}_{(i-1)i}.\mathbf{z}_{3}=\mathbf{w}_{i}$: contains the witness for the instruction computation

The GR-R1CS structure $\mathfrak{S}^{\prime} = (\mathbf{A}^{\prime}, \mathbf{B}^{\prime}, \mathbf{C}^{\prime})$ of $\mathbb{x}^{\star}_{(i-1)i}$ encodes the following constraints:
- $\overline{\mathrm{macs}}_{\mathrm{i}}=\mathrm{macs}_{i-1}$
- $\mathrm{macs}^{\prime}_{i-1}$ and $\mathrm{macs}_{i}^{\star}$ are related through the memory consistency constraints in [Memory consistency check](#eq:memory_consistency_check)
- $\overline{\mathrm{reg}}_i=\mathrm{reg}_{i-1}, \overline{\mathrm{pc}}_i=\mathrm{pc}_{i-1}$

### 3.2 Description of Generic Construction of pvRAM

In this section, we present the construction of pvRAM by protocol $\mathit{\Pi}_{\text{pvr}}$.

Let $\mathrm{pp}$ denote the public parameters. Let $\mathcal{F}=\left\{F_{j}^{\prime}\right\}_{j \in[T]}$ be a set of T instructions that can be represented by PLONK structures $\mathcal{F}_{\text{struct}}=\left\{\mathrm{plkst}_{j}^{\prime}\right\}_{j \in[T]}$. Given:
- An output register $\mathrm{reg}_{\text{out}}$
- A commitment tuple $\llbracket \mathrm{reg}_{\text{in}} \rrbracket_{\text{cki}}$ to a secret input register $\mathrm{reg}_{\text{in}}$ with commitment key $\mathrm{cki}$

The prover engages in an interactive argument with the verifier to prove that $\mathrm{reg}_{\text{out}}$ is indeed the result of executing the RAM program for N steps with instruction set $\mathcal{F}$ on the committed input register $\mathrm{reg}_{\text{in}}$. This problem statement is formally captured by the relation $\mathcal{R}_{\mathrm{ram}}$ defined in equation below.

$$
\begin{equation*}
\mathcal{R}_{\mathrm{ram}}=\left\{\left(\mathrm{cki}, \mathcal{F}_{\text{struct}}, \llbracket \mathrm{reg}_{\text{in}} \rrbracket_{\mathrm{cki}}, \mathrm{reg}_{\text{out}}\right) \mid \llbracket \mathrm{reg}_{\text{in}} \rrbracket_{\mathrm{cki}} \in \mathcal{R}_{\text{com}} \wedge \operatorname{RAM}\left(\mathrm{reg}_{\text{in}}\right)=\mathrm{reg}_{\text{out}}\right\}
\end{equation*}
$$

where $\mathcal{F}_{\text{struct}}=\left\{\text{plkst}_{j}^{\prime}\right\}_{j \in[T]}$ and $\mathrm{RAM}$ is an $N$-step RAM program and with instruction set $\mathcal{F}$ s.t. $\mathrm{RAM}$ takes as secret input $\mathrm{reg}_{\text{in}}$ and returns $\mathrm{reg}_{\text{out}}$.

**Construction 3: RAMenPaSTA as Protocol** $\mathit{\Pi}_{\mathrm{pvr}}$

With the parameters $m_{1}, \ldots, m_{5}, n$, $m_{1}^{\prime}, \ldots, m_{5}^{\prime}, n^{\prime}$ and
$$
\mathrm{pp}=\left(\mathrm{tck}, \mathrm{tck}^{\prime}\right)=\left(\left(\mathrm{ck}_{1}, \ldots, \mathrm{ck}_{5}, \mathrm{cke}\right),\left(\mathrm{ck}_{1}^{\prime}, \ldots, \mathrm{ck}_{3}^{\prime}, \mathrm{cke}^{\prime}\right)\right)
$$
satisfying $\mathrm{ck}_{1}^{\prime}=\mathrm{ck}_{2}$ and $\mathrm{ck}_{2}^{\prime}=\mathrm{ck}_{1}$. The protocol $\mathit{\Pi}_{\mathrm{pvr}}$ runs as follows: 

$\Pi_{\mathrm{pvr}}\left(\mathrm{pp}, \mathrm{cki},\left(\mathrm{ckm}_j\right)_{j \in[T]}, \mathcal{F}_{\mathrm{struct }}, \llbracket \mathrm{reg}_{\mathrm{in }} \rrbracket_{\mathrm{cki}}\right.$, $\mathrm{reg}_{\mathrm{out }} ;\left(\mathbf{z}_{i j}\right)_{i \in[N], j \in[3]},\left(\mathrm{mul}_j\right)_{j \in[T]}) \rightarrow\{0,1\}$:

1. Both parties run protocols

$$
\begin{aligned}
& \llbracket \mathbf{z}_{i j} \rrbracket_{\mathrm{ck}_{j}} \leftarrow \mathit{\Pi}_{\mathrm{com}}\left(\mathrm{ck}_{j} ; \mathbf{z}_{i j}\right) \forall i \in[N], \forall j \in[3], \\
& \llbracket \mathbf{0}^{n} \rrbracket_{\text{cke}} \leftarrow \mathit{\Pi}_{\text{com}}\left(\mathrm{cke} ; \mathbf{0}^{n}\right), \quad \llbracket \mathbf{0}^{n^{\prime}} \rrbracket_{\mathrm{cke}^{\prime}} \leftarrow \mathit{\Pi}_{\text{com}}\left(\mathrm{cke}^{\prime} ; \mathbf{0}^{n^{\prime}}\right), \\
& \llbracket \mathbf{0}^{3} \rrbracket_{\text{ck}_5} \leftarrow \mathit{\Pi}_{\text{com}}\left(\mathrm{ck}_5 ; \mathbf{0}^{3}\right), \quad \llbracket \mathrm{mul}_j \rrbracket_{\mathrm{ckm}_j} \leftarrow \mathit{\Pi}_{\text{com}}\left(\mathrm{ckm}_j ; \mathrm{mul}_j\right) \forall j \in[T]
\end{aligned}
$$
where committing to $\mathbf{0}^{n}, \mathbf{0}^{n^{\prime}}, \mathbf{0}^{3}$ by cke, cke', cke ${}_{5}$, respectively can be done locally and independently by each party.

2. Verifier: $\mathrm{gchal} \leftarrow \mathrm{CH}_{\text{global}}$ and send $\mathrm{gchal}$ to prover.

3. Each party determines $\mathfrak{S}=(\mathbf{A}, \mathbf{B}, \mathbf{C}), \mathfrak{S}^{\prime}=\left(\mathbf{A}^{\prime}, \mathbf{B}^{\prime}, \mathbf{C}^{\prime}\right)$ from $\mathrm{gchal}$.

4. Prover:
- Determine $\left(\mathbf{z}_{i 4}, \mathbf{z}_{i 5}\right)_{i \in[N]}$ from $\left(\mathbf{z}_{i 1}, \mathbf{z}_{i 2}, \mathbf{z}_{i 3}\right)_{i \in[N]}$ and $\mathrm{gchal}$.
- Find arbitrary $\left\{\mathbf{z}_{i}^{\prime}\right\}_{i \in[3]}$ s.t. $\mathbf{z}_{i}^{\prime} \in \mathbb{F}^{m_{i}^{\prime}}$ and $\mathbf{A}^{\prime} \cdot \mathbf{z}^{\prime} \circ \mathbf{B}^{\prime} \cdot \mathbf{z}^{\prime}=\mathbf{C} \cdot \mathbf{z}^{\prime}$ where $\mathbf{z}^{\prime}=\left(1\left\|\mathbf{z}_{1}^{\prime}\right\| \mathbf{z}_{2}^{\prime} \| \mathbf{z}_{3}^{\prime}\right)$.

5. Both parties run $\llbracket \mathbf{z}_{j}^{\prime} \rrbracket_{\mathrm{ck}_{j}^{\prime}} \leftarrow \mathit{\Pi}_{\mathrm{com}}\left(\mathrm{ck}_{j}^{\prime} ; \mathbf{z}_{j}^{\prime}\right),  \forall j \in[3]$.

6. $\forall i \in[N]$, both parties form tuple $\llbracket \mathbb{z}_{(i-1)i} \rrbracket_{\mathrm{pp}}$, of the form [#](./conditional-folding-scheme.html#eq:structure-of-valid-CF-pair), by setting
$$
\begin{aligned}
& \mathrm{lt}_{(i-1) i}:=i-1, \mathrm{rt}_{(i-1) i}:=i, \\
& \llbracket \mathbf{i}_{(i-1) i} \rrbracket_{\mathrm{ck}_{1}}:=\llbracket \mathbf{z}_{i 1} \rrbracket_{\mathrm{ck}_{1}}, \llbracket \mathbf{o}_{(i-1) i} \rrbracket_{\mathrm{ck}_{2}}:=\llbracket \mathbf{z}_{i 2} \rrbracket_{\mathrm{ck}_{2}}, \\
& \llbracket \mathbb{x}_{(i-1) i} \rrbracket_{\mathrm{tck}}:=\left(1,1, \llbracket \mathbf{z}_{i 1} \rrbracket_{\mathrm{ck}_{1}}, \ldots, \llbracket \mathbf{z}_{i 5} \rrbracket_{\mathrm{ck}}, \llbracket \mathbf{0}^{n} \rrbracket_{\mathrm{cke}}\right), \\
& \llbracket \mathbb{x}_{(i-1) i}^{\star} \rrbracket_{\mathrm{tck}^{\prime}}:=\left(1,1, \llbracket \mathbf{z}_{i 1}^{\prime} \rrbracket_{\mathrm{ck}_{1}^{\prime}}, \ldots, \llbracket \mathbf{z}_{i 3}^{\prime} \rrbracket_{\mathrm{ck}_{3}^{\prime}}, \llbracket \mathbf{0}^{n^{\prime}} \rrbracket_{\mathrm{cke}^{\prime}}\right), \\
& \llbracket \mathbf{s}_{(i-1) i} \rrbracket_{\mathrm{ck}_{5}}:=\llbracket \mathbf{z}_{i 5} \rrbracket_{\mathrm{ck}_{5}}, \quad \llbracket \mathbf{a}_{(i-1) i} \rrbracket_{\mathrm{ck}_{5}}:=\llbracket \mathbf{0}^{3} \rrbracket_{\mathrm{ck}_{5}}
\end{aligned}
$$

where $\llbracket \mathbb{x}_{(i-1) i} \rrbracket_{\text{tck}}$ and $\llbracket \mathbb{x}_{(i-1) i}^{\star} \rrbracket_{\text{tck}}$ are of the forms [structure of valid computation pair](./conditional-folding-scheme.html#eq:structure-of-valid-computation-pair) and [structure of valid io connection pair](./conditional-folding-scheme.html#eq:structure-of-valid-io-connection-pair), respectively.

7. Both parties run $\mathit{\Pi}_{\mathcal{CF}_{\mathrm{gnr}}}(\mathrm{HS}, \mathrm{pp},\left\{\llbracket \mathbb{z}_{(i-1) i} \rrbracket_{\mathrm{pp}}\right\}_{i \in[N]} ; \mathrm{sec}) \rightarrow\{0,1\}$ defined in [CF-generic](./conditional-folding-scheme.html#eq:cf-gnr) to obtain $\llbracket \mathbb{z}_{0 N} \rrbracket_{\mathrm{pp}}$ and run $\mathrm{CF.Prove}$ as a ZKAoK $\mathit{\Pi}_{\mathrm{pvr-prf}}$ for showing

$$
\left(\mathrm{pp}, \mathrm{cki},\left(\mathrm{ckm}_{j}\right)_{j \in[T]}, \llbracket \mathrm{reg}_{\mathrm{in}} \rrbracket_{\mathrm{cki}}, \mathrm{reg}_{\mathrm{out}},\left(\llbracket \mathrm{mul}_{j} \rrbracket_{\mathrm{ckm}_{j}}\right)_{j \in[T]}, \llbracket \mathbb{z}_{0 N} \rrbracket_{\mathrm{pp}}\right) \in \mathcal{R}_{\mathrm{pvr}-\mathrm{prf}}^{\mathfrak{S}, \mathfrak{S}^{\prime}} .
$$

Given a secure homomorphic commitment scheme $\mathcal{C}$ and HVZKAoK/HVZKPoK $\mathit{\Pi}_{\mathrm{pvr}-\mathrm{prf}}$ for relation $\mathcal{R}_{\text{pvr-prf}}^{\mathfrak{S}, \mathfrak{S}^{\prime}}$ , the protocol achieves HVZKAoK/HVZKPoK for relation $\mathcal{R}_{\text{ram}}$ with soundness error $\mathcal{O}(n_{\mathrm{plk}} \cdot(N+T) /|\mathbb{F}|+\operatorname{serr}_{\mathrm{pvr}-\mathrm{prf}}(\mathrm{pp})+\operatorname{negl}(\lambda))$.

The efficiency of the protocol is characterized by:
- Communication cost: $\mathcal{O}(N \cdot(\mathrm{c}(m_i)+...+\mathrm{c}(m_5)+\mathrm{c}(n)+\mathrm{c}(m_3')+\mathrm{c}(n')) + T \cdot \mathrm{c}(1) + \mathrm{p})$
- Prover time: $\mathcal{O}(\log T \cdot \mathrm{tp}_1 + \log N \cdot \mathrm{tp}_2 + \mathrm{tp})$ when parallelized across $\mathrm{max}\{N,T\}$ threads
- Verifier time: $\mathcal{O}(\log T \cdot \mathrm{tv}_1 + \log N \cdot \mathrm{tv}_2 + \mathrm{tv})$ when parallelized

The protocol can be instantiated using $\Sigma$-protocols, dual-mode NIWIs, or MPC-in-the-Head techniques.