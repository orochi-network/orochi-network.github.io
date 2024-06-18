## Execution Trace

We view a memory \\(M\\) to be an array \\(M=(M_1,M_2,\dots,M_m)\\). For a computation with \\(N\\) steps, n each step of a program execution, we could either i) Read a value from \\(M_i\\) for some \\(i \leq m\\) or ii) Write a value \\(val\\) to a cell \\(M_i\\) for some \\(i \leq m\\). To capture this reading/writing step, we define an execution trace to be the following tuple

$$(addr,time,op,val)$$

Here, \\(addr\\) is the address in the cell in the reading/writing process which is in \\(\\{1,2\dots,m\\}\\), \\(time\\) is the time log, \\(op \in \\{0,1\\}\\) is the value which determines whether the operation is READ or WRITE, and \\(val\\) is the value which is written to/read from \\(M*{addr}\\). We can see that, in the \\(i\\)-th execution step, the information from the \\(i\\)-th trace is sufficient to determine the action in reading/writing the value from/to the memory in that step. In the end, after the execution, we obtain we obtain an array of execution traces \\((addr_i,time_i,op_i,val_i)*{i=1}^N\\). In the next section, we show how to prove memory consistency given the whole execution trace array.
