## Memory Consistency Constraints

For an execution trace \\(tr=(addr_i,t_i,op_i,val_i)_{i=1}^n\\), one known method to prove the correctness of \\(tr\\) is to use the **sorting technique**. At a high level, we create a new trace \\(tr'\\), which is a sorted version of \\(tr\\). The elements of \\(tr'\\) is sorted in **increasing order** by its address, then time log.  We prove that the elements of \\(tr'\\) satisfy some specified constraints, then prove that \\(tr\\) and \\(tr'\\) are permutation of each other.

First, let us delve into the detail of the constraints in \\(tr'\\). The constraints are as follows:

\begin{equation}
\begin{cases}
   time_i time_{i+1}\\
   (op_{i+1}=1) \lor (addr_i'=addr_{i+1}')  \\
   (addr_i' addr_{i+1}' ) \lor ((addr_i' addr_{i+1}') \land (time_i'  time_{i+1}'))\\
   (op_{i+1}=1) \lor (val_i'=val_{i+1}') \lor (addr_{i}' \neq addr_{i+1}')\\
\end{cases}
\end{equation}

Let us explain the above constraints: Recall that in memory consistency, we simply need to prove that, for each memory cell, the value of the current step is equal to the value in the last step it was written. With this idea, we need to