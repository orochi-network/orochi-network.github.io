### DKG Security Properties
As in {{#cite GJKR99}}, we wish a \\((t,n)\\) DKG protocol to have the following properties:

**Correctness:** 
1. There is an efficient algorithm that, given any \\(t+1\\) shares provided by honest parties, output the same unique secret key \\(sk\\).
1. All honest parties agree on the same value of the public key \\(pk=g^{sk}\\).
1. \\(sk\\) is uniformly distributed in \\(\mathbb{Z}_p\\).

**Secrecy:**: For any adversary who controls at most \\(t\\) participants, he cannot learn any additional information except the value \\(pk=g^{sk}\\).
