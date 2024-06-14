## Memory Consistency Constraints

For an execution trace \\(tr=(addr_i,t_i,op_i,val_i)_{i=1}^n\\), one known method to prove the correctness of \\(tr\\) is to use the **sorting technique**. At a high level, we create a new trace \\(tr'\\), which is a sorted version of \\(tr\\). The elements of \\(tr'\\) is sorted in **increasing order** by its address, then time log.  We prove that the elements of \\(tr'\\) satisfy some specified constraints, then prove that \\(tr\\) and \\(tr'\\) are permutation of each other.
