
### Threshold ECDSA for Ed25519

 - **ed25519:** The curve ed25519 is standardized in [RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032). The curve is popular due to its high speed compared to other curves without sacrificing security. The curve parameters \(E: ax^2+y^2=1+bx^2y^2\) defined over \(\mathbb{F}_p\) with order \(n\) are as follows:
 - \(p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffed\)
 - \(a=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffec\)
 - \(b=0x52036cee2b6ffe738cc740797779e89800700a4d4141d8ab75eb4dca135978a3\)
 - \(n=	0x1000000000000000000000000000000014def9dea2f79cd65812631a5cf5d3ed\)
 
 For security and efficiency analysis,
 Thus, our implelemtation also intend to support this curve. 