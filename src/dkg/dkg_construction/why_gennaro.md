### Why Gennaro et al's Construction?

Despite there are numerous constructions for DKG, namely {{#cite GJKR99}}, there is a reason we choose the DKG protocol of Gennaro et al. 

Previously, Feldman was the first to propose a DKG construction. However, Gennaro et al. proved that in Feldman's DKG, an attacker can manipulate the result of the secret key, making it not uniformly distributed.

Canetti et al. give a construction of a DKG that is secure against an adaptive adversary. However, his construction has worse performance than {{#cite GJKR99}}, since each participant has to use Pedersen's VSS two times.