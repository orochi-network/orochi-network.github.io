## Plonk's Arithmetization

PlonK's arithmetization breaks the circuit into a batch of gates, namely, multiplications, additions, multiplications with constants and additions with constants. For each gate, the operation is transformed into a common form with respective selectors, uniquely determined by the gate. On the other hand, since breaking circuit into gates introduces the inconsistencies among wires, we additionally apply copy constraints to wires to guarantee that such incosistencies unavailable.

We first discuss how to break the circuit into gates and label wires in Section <span style="color:red">Add info here</span>. Then we present the copy constraints in Section <span style="color:red">Add info here</span>.