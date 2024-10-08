# Nova and SuperNova

For Nova variants {{#cite KS22}}, we don't commit the execution trace to a polynomial, instead we commit value of each Memory cell \\(M[add_j]\\) in steps circuit to prove the memory consistency.

Given an initial memory \\(M\\) and a execution trace list \\(tr\\): 

$$ (addr_i,instruction_i,value_i)_{i=1}^N$$

- \\(\text{instruction}_i=0\\), read \\(value_i\\) from \\(M[addr_i]\\)
- \\(\text{instruction}_i=1\\), write \\(value_i\\) to \\(M[addr_i]\\)

Every step \\(j^{th}\\) must satisfy following constraints:

1. \\(add_j \Leftarrow |M|\\)
2. \\((instruction_j - 1)\dot(M_j[add_j] - value_j) = 0\\)
3. \\(instruction_j \in \\{ 0,1 \\}\\)

In the implementation, we let \\(z_i\\) to be the \\(i^{th}\\) memory state, and the circuit has a witness input, the \\(i^{th}\\) trace record, consisting of \\(addr_i\\), \\(instruction_i\\) and \\(value_i\\). We also introduce the commitment to the memory at the last cell of \\(z_i\\) in application where one need to commit to the memory before proving.