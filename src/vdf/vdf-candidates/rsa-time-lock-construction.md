### Group of Unknown Order based Constructions

Most constructions require the prover to calculate the value \\(y=g^{2^t}\\) and provide a proof \\(\pi\\) of \\(y\\) that allows efficient verification. Without the knowledge of \\(|G|\\), calculating \\(y\\) requires \\(O(t)\\) steps.

There are two candidates for such group: The **RSA group** and the **Class Group of Imaginary Quadratic Number Field**.

The RSA group \\((\mathbb{Z},.)\\) where \\(N\\) is a product of two primes with unknown factorization. The problem of the RSA group is that we require a trusted party to generate \\(N\\).

To avoid trusted setup, the Class Group of Imaginary Quadratic Number Field is considered. For a large positive integer \\(d \equiv 3 \pmod{4}\\), 

### Pietrzak's Construction

In the construction, we assume that \\(t\\) is a power of two.

**\\(\mathsf{Gen}(1^\lambda)\\)**: The algoritms outputs  \\(pp=(G,H)\\), where: 

- \\(G\\) is a finite group.  

- \\(H\\), a hash function that maps arbitary input length to a bit string.

**\\(\mathsf{Eval}(X,pp,t)\\)**: To generate the proof for \\(Y=g^{2^t}\\), compute \\(u_{i+1}=u_i^{r_i+2^{t/2^i}}\\), where \\(r_i=H(u_i,t/2^i,v_i,u_i^{2^{t/2^i}})\\) and \\(v_i=u_i^{r_i2^{t/2^i}+2^t}\\). The algorithm outputs \\((Y,\pi)\\)=\\((g^{2^t},\\{u_1,u_2,...,u_{log(t)}\\})\\).

**\\(\mathsf{Verify}(X,Y,\pi,pp,t)\\)**: To verify the output, compute \\(v_i=u_i^{r_i2^{t/2^i}+2^t}\\) and check if \\(v_{log(t)}==u_{log(t)}^2\\). The verification time is \\(\mathcal{O}(\lambda log(t))\\).

### Wesolowski's Construction

Wesolowski introduced constructed a trapdoor VDF.

**\\(\mathsf{Gen}(1^\lambda)\\)**: The algoritms outputs \\((pk,sk)\\)=\\(((G,H_1,H_2),|G|)\\) where:  

- \\(G\\) is a finite group. 

- \\(H_1\\) is a hash function that maps a bit string \\(X\\) to an element \\(g\\) in \\(G\\).

- \\(H_2\\) is a hash function that maps arbitary input length to a bit string.

**\\(\mathsf{Eval}(X,sk,t)\\)**: By converting \\(X\\) to an element \\(g=H_1(X) \in G\\), the algorithm outputs \\((Y,\pi)\\)=\\((g^{2^t},g^{(2^t-r)/l})\\), where \\(l=H_2(g||Y)\\). The trapdoor information makes calculation faster as follow: 

- **Eval with trapdoor**: With the knowledge of \\(|G|\\) as a trapdoor, the value of \\(Y\\) can be computed within \\(\mathcal{O}log(t)\\) time by calculating \\(e=2^t \pmod{|G|}\\) and \\(Y=g^e\\). Similarly, one can compute \\(\pi\\) quickly by calculating \\(q=(2^t-r)/l \pmod{|G|}\\) and \\(\pi=g^q\\).

- **Eval without trapdoor**: Without the knowledge of \\(|G|\\), computation of \\(Y\\) and \\(\pi\\) require \\(\mathcal{O}(\lambda t)\\) and \\(\mathcal{O}(t\log(t))\\) time, respectively.  

**\\(\mathsf{Verify}(X,Y,\pi,pk,t)\\)**: To verify the output, simply check if \\(g^r\pi^l==Y\\). The verification time is \\(\mathcal{O}(\lambda)\\)

