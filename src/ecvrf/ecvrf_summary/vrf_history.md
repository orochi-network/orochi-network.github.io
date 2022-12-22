 


### History of Verifiable Random Function
Verifiable Random Function is introduced by Micali, Rabin and Vadhan {{#cite MRV99}}. They defined a Verifiable Random Function (VUF) and gave a construction based on the RSA assumption {{#cite MRV99}}, then proved that a VRF can be constructed from a VUF {{#cite MRV99}}. 

In 2002, Lysyanskaya {{#cite Lys02}} also followed the same path by constructing a VUF instead VRF. However, Lysyanskaya's VUF is based on Diffie-Hellman assumption instead, and it is the first construction that is based on Diffie-Hellman assumption. 

In 2005, Dodis and Yampolsky {{#cite DY05}} gave a direct and efficient construction using bilinear map, then improved. Later, during the 2010s, many constructions {{#cite HW10,BMR10,Jag15}} also used bilinear map, all of them used non standard assumptions to prove the security of their VRF. 

In 2015, Hofheinz and Jager {{#cite HJ15}} constructed a VRF that is based on a constant-size complexity assumption, namely the \\((n-1)\\)-linear assumption. The \\((n-1)\\)-linear assumption is not easier to break as \\(n\\) grows larger, as opposed to all the assumptions used by previous constructions {{#cite HS07}}.

In 2017, {{#cite PWHVNRG17}} construct an efficient VRF based on elliptic curve that does not use bilinear map to produce output and verify, instead they only use hash functions and elliptic curve operations. In the other hand, their hash function is viewed as random oracle model in the construction.

In 2019, Nir Bitansky {{#cite Bit19}} showed that VRF can be constructed from non-interactive witness-indistinguishable proof (NIWI).

In 2020, Esgin et al {{#cite EKSSZSC20}} was the first to construct a VRF based on lattice based cryptography, which is resistant to attack from quantum computers.

