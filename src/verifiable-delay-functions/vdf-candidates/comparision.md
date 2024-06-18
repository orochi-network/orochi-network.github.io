### Comparision

Now that we have described several well known VDF constructions, we would like to compare them. The table below compares the evaluation time, proof size and verification time of the VDF we described. Note that the value \\(P\\) in the table denote the number of processors.

| Construction   | Eval Time                             | Eval Time (parallel)                           | Verification Time         | Proof Size                   |
| -------------- | ------------------------------------- | ---------------------------------------------- | ------------------------- | ---------------------------- |
| Dwork and Naor | \\(\mathcal{O}(t)\\)                  | \\(\mathcal{O}(t^{2/3})\\)                     | \\(\mathcal{O}(1)\\)      | \\(\mathcal{O}(\lambda)\\)   |
| Pietrzak       | \\(\mathcal{O}(t+\sqrt{t})\\)         | \\(\mathcal{O}(t+\frac{\sqrt{t}}{P})\\)        | \\(\mathcal{O}(\log t)\\) | \\(\mathcal{O}(\log t)\\)    |
| Wesolowski     | \\(\mathcal{O}(t+\frac{t}{\log t})\\) | \\(\mathcal{O}(t+\frac{\sqrt{t}}{P \log t})\\) | \\(\lambda^4\\)           | \\(\mathcal{O}(\lambda^3)\\) |
| Feo et al.     | \\(\mathcal{O}(t)\\)                  | \\(\mathcal{O}(t)\\)                           | \\(\lambda^4\\)           | \\(\mathcal{O}(\lambda)\\)   |
