


### ECVRF Auxiliary Functions

 In this section, we describe the construction of \\(HashToCurve\\) and \\(HashPoint\\) in the Internet-Draft of irtf. More details can be found in [irtf-vrf08](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-vrf-08). 

 **\\(\mathsf{HashToCurve}(X,pk)\\):** Given two group elements \\(X, pk \in \mathbb{G}\\), the function output a hash value in \\(\mathbb{Z}_p\\) as follow:

Let \\(ctr=0\\)

Compute \\(pkstr=\mathsf{PointToString}(pk)\\)

Define \\(H\\) to be **"INVALID"**

While \\(H\\) is **"INVALID"** or \\(H\\) is the identity element of the group:

- Compute \\(ctrstr=\mathsf{IntToString}(ctr)\\)

- Compute \\(hstr=\mathsf{SHA256}\\)( "0x01" || "0x01" || \\(pkstr\\) || \\(X\\) || \\(ctrstr\\) || "0x00")

- Compute \\(H\\)=\\(\mathsf{StringToPoint}(hstr)\\)

- Increment \\(ctr\\) by \\(1\\)

Output \\(H\\).

 **\\(\mathsf{HashPoint}(P_1,P_2,...,P_n)\\):** Given n elements in  \\(\mathbb{G}\\), the hash value is computed as follow:

Initialize \\(str=\\)""

For \\(i=1,2,...,n\\):

- Update \\(str= str || \mathsf{PointToString}(P_i)\\)
    
Update \\(str=\mathsf{SHA256}(str)\\)

Compute \\(c=\mathsf{StringToInt}(str)\\)

Output \\(c\\)

The function \\(\mathsf{PointToString}\\) converts a point of an elliptic curve to a string. It is specified in section 2.3.3 of [SECG1]

The function \\(\mathsf{StringToPoint}\\) converts a string to a point of an elliptic curve. It is specified in section 2.3.4 of [SECG1]

