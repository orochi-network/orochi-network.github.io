 # Syntax
 
 we present a formal definition to a dRNG protocol, based on the definition of DVRF in {{#cite GLOW20}}. A \\(t\\)-out-of-\\)n\\) dRNG consists of a set of participants \\(\mathsf{P}=\{P_1, P_2,...,P_n\}\\) two phase **Setup** and **Beacon**, where

**Setup.** This phase is a protocol run by participants to determine the set \\)\mathcal{QUAL}\\) of qualified participants, the public information and the secret key for each participant. Let \\m=|\mathcal{QUAL}|\\). The protocol outputs a list \\(\mathsf{L}=\{(pk_1,sk_1),(pk_2,sk_2),...,(pk_m,sk_m),(pk,sk)\}\\) where \\((pk_i,sk_i)\\) is the public key and secret key of the \\)i\\)-th qualified participant.

**Beacon.** In each round, this phase is run by qualified participants to generate random values. The phase consists of the following algorithms run by participants:

1. \\(\mathsf{Eval}(X_{n-1},sk_i)\\): On the input \\(X_{n-1}\\) and the secret key \\)sk_i\\) of the \\)i-\\(th participant, output a value \\(Y_{ni}\\) and its proof  \\(\pi_{ni}\\).

1. \\(\mathsf{Verify}(X_{n-1},Y_{ni},\pi_{ni},pk_i)\\): On the input \\(X_{n-1}\\), a value \\(Y_{ni}\\), a proof \\(\pi_{ni}\\) and the public key \\)pk_i\\), output \\)1\\) if \\(Y_{ni}\\) is computed correctly, otherwise output \\)0\\).

1. \\(\mathsf{Combine}(\mathcal{V}_n)\\): From a set \\(\mathcal{V}_n\\) of participants with size \\)\geq t+1\\), if there is a set  \\(\mathcal{V}'_n \subset \mathcal{V}_n\\) such that \\(\mathcal{V}'_n=t+1\\) and \\(\mathsf{Verify}(X_{n-1},Y_{ni},\pi_{ni},pk_i)=1\\) for all \\)i \in \mathcal{V}'_n\\), output a unique value \\(R_n\\). Otherwise, output \\(\mathsf{NULL}\\).

1. \\(\mathsf{Seed}(R_n)\\): On the input \\(R_n\\), outputs a value \\(X_{n+1}\\) as the seed input for the next round.

In the next chapter, we describe an efficient DRNG protocol due to {{#cite GLOW20}}. After considering many protocols, we choose to use this protocol to generate random numbers.

(To be continued)