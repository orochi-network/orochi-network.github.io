# Poseidon permutation design

At the heart of the \\(\mathsf{Poseidon}\\) hash is the \\(\mathsf{Hades}\\)-based permutation \\(\mathsf{Poseidon}^\pi\\). As mentioned before, \\(\mathsf{Hades}\\) employs efficient *round functions* which is applied many times in order ti make the permutation behave like a random permutation. The same round function is applied throughout the permutation to destroy its symmetries and structural properties.

This section explains in detail the \\(\mathsf{Hades}\\) [design strategy](./hades-design-strategy.md), the [detailed design](./hades-based-design.md) of \\(\mathsf{Hades}\\)-based permutation, and the [different instantiations](./concrete-poseidon-instantiation.md) of \\(\mathsf{Poseidon}^\pi\\) permutation in different use cases.