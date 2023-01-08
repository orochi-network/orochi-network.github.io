### Why Gennaro et al's Construction?

Despite there are numerous constructions for DKG, namely {{#cite GJKR99}}, there is a reason we choose the DKG protocol of Gennaro et al. 

Previously, Feldman was the first to propose a DKG construction. However, Gennaro et al. proved that in Feldman's DKG, an attacker can manipulate the result of the secret key, making it not uniformly distributed.

Canetti et al. give a construction of a DKG that is secure against an adaptive adversary. However, his construction has worse performance than Gennaro's, since each participant has to use Pedersen's VSS two times. In addition, no adaptive
adversary has been able to successfully attack the construction of Gennato et al.

Numerous attempts have been made to reduce the communication cost for a DKG {{#cite KG09, CS04, TCZAPGD20}}. However, all these schemes require a trusted party. This quite contradict the goal of a DKG.

This make the DKG construction of Gennaro et al. remains a simple, efficient and secure DKG protocol.