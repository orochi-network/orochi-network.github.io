### DKG Security Properties
As in {{#cite GJKR99}}, we wish a \\((t,n)\\) DKG protocol to have the following properties:
**Correctness:** 
1. All subsets of \\(t+1\\) shares provided by honest parties defines the same unique secret key \\(sk\\).
1. All honest parties have the same value of the public key \\(pk=g^{sk}\\).
1. \\(sk\\) is uniformly distributed in \\(\mathbb{Z}_p\\).

**Secrecy:**: No additional information is learned by adversary except the value \\(pk=g^{sk}\\).
