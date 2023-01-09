### Gennaro et al's Construction
The DKG protocol consists of two phases, namely, generating and extracting, working as follows:

**Public Parameters:** Let \\(p\\) be a prime number. Let \\(G\\) be a cyclic group of order \\(p\\) with generators \\(g\\) and \\(h\\). The public parameters of the system are \\(p,G,g,h\\).

**Generating:** This process works as follows:
1. Each participant \\(P_i\\) chooses two random polynomials \\(f_i(z)=a_{i0}+a_{i1}z+...+a_{it}z^t\\) and \\(f_i'(z)=b_{i0}+b_{i1}z+...+b_{it}z^t\\) and broadcasts \\(C_{ij}=g^{a_{ij}}h^{b_{ij}}\\) for \\(j=0,1,...,t\\).
1. The participant \\(P_i\\) then sends \\(s_{ij}=f_i(j)\\) and \\(s'_{ij}=f_i'(j)\\) to \\(P_j\\).
1. Each participant \\(P_j\\) verifies the shares he received from each \\(P_i\\) by checking whether
  
$$g^{s_{ij}}h^{s_{ij}'}\stackrel{?}{=} \prod_{k=0}^{t}C_{ik}^{j^k}. (*)$$

If the check fails for some \\(i\\), \\(P_j\\) complains against \\(P_i\\).
1. Each \\(P_i\\) who receives a complaint from \\(P_j\\) broadcasts \\(s_{ij}\\) and \\(s_{ij}'\\) that satisfy Equation \\((*)\\).
1. A participant \\(P_i\\) is disqualified if he receives at least \\(t+1\\) complaints or answers a complaint with value that does not satisfy Equation. Then a set \\(\mathcal{QUAL}\\) of qualified participants is determined.
1. For each \\(i\\), the secret key \\(sk_i\\) of \\(P_i\\) is equal to \\( \sum_{j\in \mathcal{QUAL}} s_{ji}\\). For any set \\(\mathcal{V}\\) of at least \\(t+1\\) participants, the secret key \\(sk\\) is equal to \\( \sum_{i \in \mathcal{V}} sk_i\cdot\lambda_{i,\mathcal{V}}\\).

**Extracting:** The process works as follows:
 
1. Each participant \\(P_i\\) in the set \\(\mathcal{QUAL}\\) publishes \\(A_{ij}=g^{a_{ij}}\\) for \\(j=0,1,2,\dots,t\\).
1. Each participant \\(P_j\\) verifies \\(A_{ij}\\) for each \\(i\\). Specifically, \\(P_j\\) checks whether
$$g^{s_{ij}}\stackrel{?}{=} \prod_{k=0}^{t}A_{ik}^{j^k}.$$
If the check fails for some \\(i\\), \\(P_j\\) complains against \\(P_i\\).
1. For each \\(i\\) that \\(P_i\\) receives at least one valid complaint, all other parties run Pedersen VSS to reconstruct \\(f_i(z)\\), and restore \\(s_{i0}\\) and \\(A_{ij}\\) for \\(j=0,1,...,t\\). The public key is equal to \\(pk= \prod_{i \in \mathcal{QUAL}}A_{i0}\\) 
1. The public key \\(pk_i\\) of \\(P_i\\) is calculated as 
\\(pk_i=g^{sk_i}=\prod_{j \in \mathcal{QUAL}}g^{s_{ji}}= \prod_{j \in \mathcal{QUAL}}\prod_{k=0}^{t}A_{jk}^{i^k}\\)

The security proof of the DKG protocol can be found in {{#cite GJKR99}}.
