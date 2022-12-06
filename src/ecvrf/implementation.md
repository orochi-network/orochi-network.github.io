
### Implementation

We implement the ECVRF described above in python and rust. We choose the curve secp256k1 instead of the curve P-256 recommended in [irtf-vrf08]. We also use the keccak hash instead of SHA256 as in [irtf-vrf08]. The functions \\(\mathsf{StringToCurve}\\) and \\(\mathsf{StringToField}\\) in [SECG01] are used. The paper [SECG01] implement these functions for a general curve, therefore it will contain many steps. However, we only need to implement some of them, since we are using the curve secp256k1.