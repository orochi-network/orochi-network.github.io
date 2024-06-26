### Introduction

Verifiable Delay Functions (VDF) were introducted by Boneh et al {{#cite BBBF18}} in 2018. Informally, a VDF is a funtion that requires a specified number of step to compute its output, even with a large number of parallel processor, but the correctness of the output can be **quickly** verified.
While there can be many functions that require a specified of step to compute its output, not all of them are qualified to become VDFs. For example, consider the function \\(f(X)=\mathsf{SHA256}^t(X)=\mathsf{SHA256}(\mathsf{SHA256}(...(\mathsf{SHA256}(X))...))\\). Calculating \\(f(X)\\) require \\(t\\) iterations, even on a parallel computer. However, verification would also require \\(t\\) iterations, thus the function \\(f(X)\\), while VDF verification time must be within \\(O(poly(\log t))\\).
VDFs has found it application in many aspects {{#cite BBBF18}}. We will highlight some of them later in the **Application** section.
