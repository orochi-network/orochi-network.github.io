### Applications

We present several applications of VDF below:

**Randomness Beacon**:
To see the application of VDF in randomness beacons, we take a look at two examples below:
In proof of work blockchains, miners find solutions to computational puzzles and then submits them for monetary rewards. However, there can be more than one solution, and a miner may submit the one that benefits him the most, or refuse to submit if it does not benefit him. Using VDF, then given the deterministic nature of VDF, a miner cannot choose any output other than the VDF output for his goal.
Another example is the the RANDAO {{#cite Ra17}} that use "commit-and-reveal" paradigm. In RANDAO, each participant \\(P_i\\) submit the commitment of a random value \\(s_i\\), after each participants reveal \\(s_i\\) and the final output is defined to be \\(s=\oplus_{i=1}^ns_i.\\). The problem is that the last participant may not submit his value if the final output does not benefit him. One approach to solve this problem is to use VDF. After all \\(s_i\\)s are revealed, the value \\(H(s_1,s_2,...,s_n)\\) is used as the seed of the VDF and the beacon output is the VDF output {{#cite LW15}}. With a sufficient time parameter, the last participant cannot predict the output on time, even if he knows the seed.

**Resource efficient Blockchain**: Various attempts have been made to develop resource-efficient blockchains to reduce energy consumption. However, resource-efficient mining suffers from nothing-at-stake attack {{#cite BBBF18}}. Cohen proposes to combine proof of resources {{#cite KCMW15}} with an incremental verifiable delay function and use the product of proven resources and
induced delay as a measure of blockchain quality. This scheme prevents nothing-at-stake attacks by simulating the proof-of-work mining process {{#cite WXWJWR22}}.
For other applications of VDF, we refer the readers to {{#cite BBBF18}}.
