
### Threshold ECDSA for Ed25519

The curve ed25519 is standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). In the paper, the  authors instatiatied the Schnorr signature scheme  with an Edward curve instead of an ordinary elliptic curve such as secp256k1. The curve is popular due to its high speed compared to other curves without sacrificing security. The curve parameters \(E: ax^2+y^2=1+bx^2y^2\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:

 - \(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\)

 - \(a=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffec\)

 - \(b=0x52036cee2b6ffe738cc740797779e89800700a4d4141d8ab75eb4dca135978a3\)

 - \(n=	0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\)

 - \(g=(0x216936D3CD6E53FEC0A4E231FDD6DC5C692CC7609525A7B2C9562D608F25D51A, 0x6666666666666666666666666666666666666666666666666666666666666658)\)
 
 For security analysis, the provable security of the instatiation of the ECDSA using the curve Ed25519 has been well studied in {{#cite BCJZ20}}. In addition, it has been confirmed in {{#cite }} that the curve indeed achieves \(128\) bit security level, which is considered secure. 
 
 For efficiency analysis, ed25519 is one of the fastest curve have been developed. More speficially, according to {{#cite }}, we can create \(109000\) signatures per second and \(71000\) signatures per second with a curve of \(128\) bit security level.
 
Although the curve ed25519 was ordinary developed to support Schnorr signature, in Canneti's construction, **we only require any group where the discrete log algorithm is hard**. Since the ed25519 group above achieves \(128\) bit security level, **we can instatiate the theshold signature construction of Canneti et al. using the ed25519 curve specified above**.  Thus, our implelemtation also intend to support this curve. 