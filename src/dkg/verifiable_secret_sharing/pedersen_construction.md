### Pedersen's Construction
We describe a VSS scheme from Pedersen {{#cite Ped91}}. This scheme provides perfect information theoretic security against any adversary with unbounded computational power.

**Share** The dealer chooses two polynomials \\(p(x)=a_0+a_1x+a_2x^2+...+a_tx^t\\) and \\(p'(x)=b_0+b_1x+b_2x^2+...+b_tx^t\\) such that \\(a_0=s\\). Then he compute \\(s_i=p(i)\\) and \\(s_i'=p'(i)\\). The dealer then broadcasts \\(C_i=g^{s_i}h^{s_i'}\\). Then he gives the party \\(P_i\\) the tuple \\((s_i,s_i')\\). This allows \\(P_i\\) to check if \\((s_i,s_i')\\) really defines a secret by checking that
$$g^{s_{i}}h^{s_{i}'}=\prod_{k=0}^{t}(C_{k})^{i^k} (1).$$
    If a party $P_i$ receives a share that does not satisfy \\((1)\\), he will complains against the dealer.
    The dealer must reveal the share \\((s_i,s_i')\\) that satisfies  for each complaining party \\(P_i\\). If any of the revealed shares does not satisfy \\((1)\\) the equation, the dealer is marked invalid.

**Reconstruct** Each participant submits  \\((s_i,s_i')\\). Everyone can verify if \\(P_i\\) submitted the correct shares by checking if \\(1\\) is satisfied. If \\(P_i\\) receives more than \\(t\\) complaints, then he is disqualified. Given \\(t+1\\) valid shares, \\(s_1,s_2,...,s_{t+1}\\) from parties \\(P_{x_1},P_{x_2},...,P_{x_{t+1}}\\), the secret \\(s\\) can be computed as: 
$$s= \sum_{i=0}^{t}s_i \left(\prod_{\substack{j=0 \\ j\neq i}}^{t}\dfrac{x_j}{x_j-x_i}\right).$$
