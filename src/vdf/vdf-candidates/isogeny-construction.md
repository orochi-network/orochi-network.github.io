isogeny-construction.md
### Isogeny based Constructions

In 2019, Luca de Feo et al {{#cite FMPS19}} constructed a VDF based on isogenies of supersingular elliptic curves and pairing. Their construction is motivated by the BLS signature, which uses a bilinear pairing. {{#cite FMPS19}}.

For more information about elliptic curves and isogeny graph \\(G_{F_q}(l)\\), we refer the reader to see  [the Elliptic Curve chapter I wrote earlier.](https://github.com/orochi-network/cookbook/tree/vdf/src/isogeny-based-crypto).

Before continuing, we introduce some notations. Let \\(l\\) be a small prime. Let  \\(N\\) be a large prime and \\(p\\) be another prime such that \\(p+1\\) is divisible by \\(N\\). For an elliptic curve \\(E/\mathbf{F}\\), define \\(E[N]=\\{P \in E | N*(P)=\mathcal{O}\\}\\) where \\(\mathcal{O}\\) is the infinity point. 

The definition and properties of  Weil pairing can be found in {{#cite BLS03}}. In the construction, the authors consider the pairing \\(e_N: E[N]\times E'[N] \rightarrow \mu_N\\). As the authors stated in their paper, the main important property the pairing is that, for any curves \\(E\\), isogeny \\(E \rightarrow E'\\) and \\(P \in E[N]\\) and \\(Q \in E'[N]\\) we have  \\(e_N(P,\hat{\phi(Q)})=e_N(\phi(P),Q)\\).

Now we move to describe the VDF Construction.

**\\(\mathsf{Gen}(1^\lambda,t)\\)**: The algorithm selects a supersingular ellpitic curve \\(E/ \mathbb{F}\\), then choose a direction of the supersingular isogeny graph of degree \\(l\\), then compute the isogeny \\(\phi E \rightarrow E'\\) of degree \\(l^t\\) and its dual isogeny \\(\hat{\phi}\\).

**\\(\mathsf{Eval}(Q)\\)**: For a point \\(Q \in E'[N]\cap E(\mathbf{F})\\), outputs \\(\hat{\phi(Q)}\\).

**\\(\mathsf{Verify}(E,E',P,Q,\phi(P),\hat{\phi(Q)})\\)**: Check that \\(\hat{\phi(Q)} \in E[N]\cap E(\mathbf{F})\\) and \\(e_N(P,\hat{\phi(Q)})=e_N(\phi(P),Q)\\).

In the construction, to compute the isogeny the map \\(\hat{\phi}\\), we are expected to walk \\(t\\) times between some vertices in the isogeny graph \\(G_{F_q}(l)\\). Currently, there is efficient algorithm to find shortcut between these vertices of the graph. 

