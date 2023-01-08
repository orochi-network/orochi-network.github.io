### Syntax and Properties

#### Syntax
A \\((t,n)\\) Verifiable Secret Sharing consists of two phases: **Share** and **Reconstruct** as follow:

**Share:** The dealer \\(D\\) has a secret value \\(s\\). Initially, there are \\(n\\) parties \\(P_1, P_2, ..., P_n\\). The sharing phase may consist of several rounds of interaction between parties. At the end
of the phase, each party \\(P_i\\) holds a share \\(s_i\\) that will be required to reconstruct the secret of the dealer later.

**Reconstruct:** In this phase, each party \\(P_i\\) publishes its share \\(s_i\\) from the sharing phase. Then, from these shares, a reconstruction function will be applied to output the original secret.

#### Properties

As in {{#cite BKP11}}, a VSS need to have the following properties:

**Correctness**: If \\(D\\) is honest, then the reconstruction function outputs \\(s\\) at the end of the **Reconstruct** phase and all honest parties agree on the value \\(s\\).

**Secrecy**: If \\(D\\) is honest, then any adversary that control at most \\(t\\) parties does not learn any information about \\(s\\) during the **Share** phase.

**Commitment**: If \\(D\\) is dishonest, then at the end of the sharing phase there exists a value \\(s^* \in \mathbb{F}_p\\) 
such that at the end of the **Reconstruct** phase all honest parties agree on \\(s^*\\).