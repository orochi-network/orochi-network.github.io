### Comparision

Now that we have described several well known VDF constructions, we would like to compare them. The table below compares the evaluation time, proof size and verification time of the VDF we described. Note that the value \\(P\\) in the table denote the number of processors.

| Construction   | Eval Time                             | Eval Time (parallel)                           | Verification Time         | Proof Size                   |
| -------------- | ------------------------------------- | ---------------------------------------------- | ------------------------- | ---------------------------- |
| Dwork and Naor | \\(O(t)\\)                  | \\(O(t^{2/3})\\)                     | \\(O(1)\\)      | \\(O(\lambda)\\)   |
| Pietrzak       | \\(O(t+\sqrt{t})\\)         | \\(O(t+\frac{\sqrt{t}}{P})\\)        | \\(O(\log t)\\) | \\(O(\log t)\\)    |
| Wesolowski     | \\(O(t+\frac{t}{\log t})\\) | \\(O(t+\frac{\sqrt{t}}{P \log t})\\) | \\(\lambda^4\\)           | \\(O(\lambda^3)\\) |
| Feo et al.     | \\(O(t)\\)                  | \\(O(t)\\)                           | \\(\lambda^4\\)           | \\(O(\lambda)\\)   |
