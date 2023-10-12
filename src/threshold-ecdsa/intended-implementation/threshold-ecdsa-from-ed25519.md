
### Threshold ECDSA for Ed25519

The curve ed25519 is standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). In the paper, the  authors instatiatied the ordinary ECDSA signature scheme  with an Edward curve. The curve is popular due to its high speed compared to other curves without sacrificing security. The curve parameters \(E: ax^2+y^2=1+bx^2y^2\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:
 - \(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\)
 - \(a=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffec\)
 - \(b=0x52036cee2b6ffe738cc740797779e89800700a4d4141d8ab75eb4dca135978a3\)
 - \(n=	0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\)
 
 For security analysis, in threshold ECDSA, we only require that the discrete log problem is hard, hence the threshold ecdsa can be safely  instatiatied using the curve ed25519. In addition, the provable security of the instatiation of the ECDSA using the curve Ed25519 has been well studied in {{#cite BCJZ20}}. 
 
 For efficiency analysis, Ed25519 is faster than most curves. More speficially, according to {{#cite }}, we can create \(109000\) signatures per second and \(71000\) signatures per second with a curve of \(128\) bit security level.
 
 Thus, our implelemtation also intend to support this curve. 