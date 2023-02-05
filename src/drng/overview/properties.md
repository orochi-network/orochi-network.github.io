# Properties

In term of security, we wish to build a protocol that satisfies the following properties:

**Pseudo-randomness**. For all adversary, \\(mathcal{A}\\), The game \\(\ExpRand_{DRNG}^{\mathcal{A}}(1^\lambda)\\) is defined as follow:

1. ***Corruption***: \\({A}\\) chooses a set \\({C}\\) with \\)|\mathcal{C}| \leq t\\) to control. \\({A}\\) acts on the behalf of corrupted participants, whereas the challenger acts on the behalf of honest participants.
1. ***Initialize***: The challenger runs the **Setup} phase by interacting with \\({A}\\). After this, the set \\(\mathcal{QUAL}\\) is defined.
1. ***Pre Challenge Queries***: \\({A}\\) is allowed to query \\({O}_{\mathsf{GetRandomness}}\\) at most \\(R\\) times.
     **Challenge}: Let \\(R\\) be the current round and \\(X_r\\) be the current input. The challenger proceed as follow:
    \begin{enumerate}
     Compute \\((Y_{ri},\pi_{ri})=Eval(X_r,sk_i)\\) for each \\)i \in \qual \backslash \mathcal{C}\\).
     Compute \\(R_r=\mathsf{Combine}(\{Y_{ri}\}_{i \in \qual})\\).
     Choose a value \\(b\\) uniformly in \\)\{0,1\}\\).
     If \\)b=0\\), then set \\(Y=R_r\\), otherwise choose \\(Y\\) uniformly in \\)\{0,1\}^{\lambda}\\) and send \\(Y\\) to \\({A}\\). 
    \end{enumerate}
1. ***Guess***: \\({A}\\) send \\(b'\\) to the challenger. \\({A}\\) succeeds in the experiment if \\(b=b'\\).

\\({O}_{\mathsf{GetRandomness}}\\): Let \\(t\\) be the current round and \\)X_t\\) be the current input. The challenger proceed as follow:
\begin{enumerate}
 Compute \\((Y_{ti},\pi_{ti})=Eval(X_t,sk_i)\\) for each \\)i \in \qual \backslash \mathcal{C}\\).
 Compute \\(R_t=\mathsf{Combine}(\{Y_{ti}\}_{i \in \qual})\\).
 Compute \\(X_{t+1}=\mathsf{Seed}(R_t)\\) and set the current round to \\)t'=t+1\\).
 Return  and \\((\{(i,Y_{ti},\pi_{ti})\}_{i \in \mathcal{G}},R_t,X_{t+1})\\) to \\({A}\\).

Informally, an adversary \\({A}\\) who controls at most \\)t\\) participants is allowed to know the first \\(R-1\\) outputs of the protocol, say \\(R_1,R_2,...,R_{r-1}\\). Then he has to distinguish the \\(R\\)-th output \\(R_r\\) from a uniformly chosen value by guessing the bit \\)b\\). If he cannot distinguish then does not know the bit \\)b\\), thus the probability he guess \\)b\\) correctly will be close to \\(1/2\\).

**Unbiasability.** Any colluding adversary or adversariesy should not be able to affect future random beacon values for their own goal.

**Availability.** Any colluding adversary or adversaries cannot delay the progress of the protocol or cause the protocol to abort.

**Public verifiability.** Any third party should be able to verify the outputs of the protocol using public information.

In term of efficiency, we wish the dRNG protocol to satisfy the following property:

**Scalability.** The Communication Cost, Computation cost per Node and Verification Cost per Verifier of the protocol are all quasi-linear in \\(n\\), where \\(n\\) is the number of participants.
