## Execution Trace

We view a memory \\(M\\) to be an array \\(M=(M_i)_{i=1}^m\\). For a computation with \\(n\\) steps, n each step of a program execution, we could either i) Read a value from \\(M_i\\) for some \\(i \leq m\\) or ii) Write a value \\(val\\) to a cell \\(M_i\\) for some \\(i \leq m\\). To capture this reading/writing step, we define an execution trace \\(tr=(tr_i)_{i=1}^n\\) where 
$$tr_i=(addr_i,time_i,op_i,val_i)$$