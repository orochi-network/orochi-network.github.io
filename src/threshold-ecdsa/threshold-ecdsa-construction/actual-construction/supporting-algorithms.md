### Supporting Protocols

In this section, we specify the supporting protocols that support the signing protocol described in the previous section.

#### Feldman's VSS

Recall that in Step 2 of the key generation protocol, each participant \\(P\\) has to perform Feldman's VSS to share his secret \\(s\\) to other participants \\(P_i\\). The process of Feldman's VSS is described as follow:

1. \\(P\\) generate a random degree \\(t\\) polynomial \\(f(x)=a_0+a_1x+\dots+a_tx^t\\) such that \\(a_0=s\\), then broadcast \\(A_i=g^{a_i}\\) for \\( i \in \\{0,1,\dots,t\\}\\). Finally \\(P\\) secretly send the share \\(s_i=f(i)\\) to the \\(i\\)-th participant \\(P_i\\). 

2. Each participant \\(P_i\\) can verify the correctness of his share \\(s_i\\) by checking \\(g^{s_i}=\prod_{j=0}^tA_j^{i^j}\\). If the check fails, \\(P_i\\) broadcasts a complaint to \\(P\\). If \\(P\\) receives a complaint he will be disqualified. 


#### Zero Knowledge Proofs

**Schnorr Protocol:**

In Step 3 of the initial key generation process, a participant who broadcasts \\(pk_i=g^{sk_i}\\) must prove the knowledge of \\(sk_i\\) using Schnorr protocol. Schnorr protocol can be described as follow:

1. The prover chooses \\(a \in \mathbb{Z}_p\\) and sends \\(\alpha=g^a\\).

2. The verifier sends a challenge \\(c \in \mathbb{Z}_p\\).

3. The prover sends \\(u=a+c\sigma\\).

4. The verifier checks if \\(g^u=\alpha\cdot pk_i^c\\).

**Schnorr Protocol for Ring:**

In Step 3.3 of the key refresh process, a participant who broadcasts \\(h_1,h_2\\) must prove that there is a value \\(s\\) such that \\(h_2=h_1^s \pmod{N}\\) The protocol can be described as follow:

1. For each \\(i=1,\dots,\lambda\\), the prover chooses \\(a_1 \in \mathbb{Z_{\phi(N)}}\\) and sends \\(A_i=h_1^{a_i} \pmod{N}\\) to the verifier.

2. The verifier sends a challenge \\(e_i \in {0,1}\\) for each \\(i=1,\dots,\lambda\\).

3. For each \\(i=1,\dots,\lambda\\), the prover sends \\(z_i=a_i+e_i s \pmod{\phi(N)}\\) and send \\(z_i\\) to the verifier.

4. The verifier checks if \\(h_1^{z_i}=A_i\cdot h_2^c \pmod{N}\\) for each \\(i=1,\dots,\lambda\\).



**Proof of Product of Two Primes:**

In Step 3.2 of the key refresh process, a participant must prove that the RSA modulus \\(N\\) is a product of two primes \\(p,q\\) such that \\(N=pq\\) and \\(p \equiv q \equiv 3 \pmod{4}\\) and \\(gcd(N,\phi(N))=1\\). The protocol process as follow:

1. The prover samples \\(w \in \mathbb{Z_N}\\) s.t \\(\left(\dfrac{w}{N}\right)=-1\\) where \\(\left(\dfrac{w}{N}\right)\\) denotes the Jacobian symbol.

2. The verifier samples \\(y_1,\dots,y_{\lambda} \in \mathbb{Z_N}\\) and send them to the prover.

3. The prover proceed as follows:

    - Set \\(x_i=(y_i')^{1/4} \pmod{N}\\) where \\(y_i'=(-1)^{a_i}w^{b_i}y_i\\) such that \\(x_i\\) is well defined.

    - Compute \\(z_i=y_i^{-N} \pmod{N}\\)

    - Send \\((x_i,a_i,b_i,z_i)_{i=1}^\lambda\\) to verifier.

4. The verifier checks that \\(N\\) is not a prime, \\(z_i^N \equiv y_i \pmod{N}\\) and \\(x_i^4 \equiv (-1)^{a_i}w^{b_i}y_i \pmod{N}\\). Accept if and only if all checks pass.

**Pallier Encryption Range Proof:** 

In Step 1.2 of the signing process, each participant given \\(K_i=\mathsf{Enc_i}(k_i)\\) has to provide a proof \\(\pi\\) certifying \\(k_i<2^{3\lambda}\\). The protocol for providing \\(\pi\\)  proceeds as follow:

1. The protocol chooses \\(N,h_1,h_2\\) to be the auxiliary set-up parameter for the protocol, where \\(N\\) is a product of two safe prime and \\(h_1,h_2\\) generate the same multiplicative group modulo \\(N\\).

2. The prover samples \\(\alpha \in [-2^{3\lambda},\dots,2^{3\lambda}]\\), \\(\delta \in [-2^{3\lambda}\cdot N,\dots,2^{3\lambda}\cdot N]\\), \\(u \in [-2^{\lambda}\cdot N,\dots,2^{\lambda}\cdot N]\\), \\(r \in \mathbb{Z_{N_1}}\\)

3. The prover computes \\(S=h_1^k h_2^u \pmod{N}\\), \\(A=(1+N_1)^\alpha r^{N_1} \pmod {N_1^2}\\) and \\(C=h_1^\alpha h_2^\delta \pmod{N}\\)

4. The prover sends \\((S,A,C)\\) to the verifier.

5. The verifier chooses a challenge \\(e \in [-p,\dots,p]\\) and sends \\(e\\) to the prover.

6. The prover computes \\(z_1=\alpha+ek\\), \\(z_2=r\rho^e \pmod{N_1}\\) and \\(z_3=\delta+eu\\)

7. The prover sends \\(\pi=(z_1,z_2,z_3)\\) to the verifier

8. The verifier checks if \\((1+N_1)^{z_1}z_2^{N_1}=AK^e \pmod{N_1^2}\\) and \\(h_1^{z_1}h_2^{z_3}=CS^e \pmod{N}\\)

9 The verifier checks that \\(z_i \in [-2^{3\lambda},\dots,2^{3\lambda}]\\)

**Proof of Paillier Encryption given Group Commitment:**

In Step 2.4 of the signing process, each participant has public input \\((C,X)\\) and secret input \\(x\\) and has to provide a proof \\(\pi\\) which proves that \\(((C,X),x) \in \mathcal{R}\\), where

\\(\mathcal{R}=\\{((C,X),(x,\rho))\ |\ X=g^{x}\ \land\ C=\mathsf{Enc_1}(x,\rho)\ \land\ x \le 2^{3\lambda}\\}\\). The protocol for providing \\(\pi\\) proceeds as follow:

1. The protocol chooses \\(N,h_1,h_2\\) to be the auxiliary set-up parameter for the protocol, where \\(N\\) is a product of two safe prime and \\(h_1,h_2\\) generate the same multiplicative group modulo \\(N\\).

2. The prover samples \\(\alpha \in [-2^{3\lambda},\dots,2^{3\lambda}]\\), \\(\delta \in [-2^{3\lambda}\cdot N,\dots,2^{3\lambda}\cdot N]\\), \\(u \in [-2^{\lambda}\cdot N,\dots,2^{\lambda}\cdot N]\\), \\(r \in \mathbb{Z_{N_1}}\\)

3. The prover computes \\(S=h_1^xh_2^u \pmod{N}\\), \\(A=(1+N_1)^\alpha r^N_1 \pmod{N_1^2}\\), \\(Y=g^\alpha\\), \\(D=h_1^\alpha h_2^\gamma \pmod{N}\\)
4. The prover sends \\(S,A,Y,D,F\\) to the verifier.
5. The verifier chooses a challenge \\(e \in [-p,\dots,p]\\) and sends \\(e\\) to the prover.
6. The prover computes \\(z_1=\alpha+ek\\), \\(z_2=r\rho^e \pmod{N_1}\\) and \\(z_3=\gamma+eu\\)
7. The prover sends \\(\pi=(z_1,z_2,z_3)\\) to the verifier.
8. The verifier checks that \\((1+N_1)^{z_1}z_2^{N_1}=AC^e \pmod{N_1^2}\\), \\(g^{z_1}=YX^e\\) and \\(h_1^{z_1}h_2^{z_3}=DS^e \pmod{N}\\)
9. The verifier check that \\(z_1 \in [-2^{3\lambda},\dots,2^{3\lambda}]\\)

**Proof of Paillier Operation given Group Commitment:**

In Step 2.5 and 2.6 of the signing process, each participant has public input \\((C,X,K,Y)\\) and secret input \\((x,y)\\) and has to provide a proof \\(\pi\\) which proves that \\(((C,X,K,Y),(x,y)) \in \mathcal{R}\\), where

\\(\mathcal{R}=\\{((C,X,K,Y),(x,y,\rho,\rho_y))\ |\ C=x\cdot K-\mathsf{Enc_1}(y,\rho)\ \land\ X=g^{x}\ \land\ Y=\mathsf{Enc_2}(y,\rho_y)\ \land\ x<2^{3\lambda}\ \land \ y \le 2^{7\lambda}\\}\\) The protocol for providing \\(\pi\\) proceeds as follow:

1. The protocol chooses \\(N,h_1,h_2\\) to be the auxiliary set-up parameter for the protocol, where \\(N\\) is a product of two safe prime and \\(h_1,h_2\\) generate the same multiplicative group modulo \\(N\\).

2. The prover samples \\(\alpha\in [-2^{3\lambda},\dots,2^{3\lambda}]\\), \\(\beta\in [-2^{7\lambda},\dots,2^{7\lambda}]\\), \\(\gamma, \delta \in [-2^{3\lambda}\cdot N,\dots,2^{3\lambda}\cdot N]\\), \\(m, u \in [-2^{\lambda}\cdot N,\dots,2^{\lambda}\cdot N]\\), \\(r \in \mathbb{Z_{N_1}}\\) and \\(r_y \in \mathbb{Z_{N_2}}\\)

3. The prover computes \\(A=K^\alpha((1+N_1)^\beta r^{N_1}) \pmod {N_1^2}\\), \\(B_x=g^\alpha\\), \\(B_y=(1+N_2)^\beta r_y\\), \\(E=h_1^\alpha h_2^\gamma \pmod{N}\\), \\(F=h_1^\beta h_2^\gamma \pmod{N}\\), \\(S=h_1^xh_2^m \pmod{N}\\), \\(T=h_1^yh_2^u \pmod{N}\\)
4. The prover sends \\(S,T,A,B_x,B_y,E,F\\) to the verifier.
5. The verifier chooses a challenge \\(e \in [-p,\dots,p]\\) and sends \\(e\\) to the prover.
6. The prover compute \\(z_1=\alpha+ex\\), \\(z_2=\beta+ey\\), \\(z_3=\gamma+em\\), \\(z_4=\delta+eu\\), \\(w=r \rho^e \pmod{N_1}\\), \\(w_y=r \rho_y^e \pmod{N_2}\\)
7. The prover sends \\(\pi=(z_1,z_2,z_3,z_4,w,w_y)\\) to the verifier.
8. The verifier checks that \\(K^{z_1}(1+N_1)^{z_2}w^{N_1} = A C^e \pmod{N}\\), \\(g^{z_1}=B_xX^e\\), \\((1+N_2)^{z_2}w_y^{N_2}=B_yY^e \pmod{N_2}\\), \\(h_1^{z_1}h_2^{z_3}=ES^e \pmod{N}\\), \\(h_1^{z_2}h_2^{z_4}=FT^e \pmod{N}\\)
9. The verifier check that \\(z_1 \in [-2^{3\lambda},\dots,2^{3\lambda}]\\) and \\(z_1 \in [-2^{7\lambda},\dots,2^{7\lambda}]\\)


#### Commitment Scheme

In Step 1 of the Key Generation protocol, we require participants to commit their messages using a commitment scheme \\(\mathsf{Com}\\). In practice, one can use a cryptographic hash function \\(\mathsf{H}\\) and define the commitment of \\(X\\) to be \\(\mathsf{H}(X,r)\\) where \\(r\\) is chosen uniformly. 