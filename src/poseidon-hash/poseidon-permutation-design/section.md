# Poseidon permutation design

Several \\(\mathsf{Poseidon}^\pi\\) instances are provided for different use cases, but all based on the described sponge function in [Section 9.1](./../cryptographic-sponge-function/cryptographic-sponge-function.md):
- Determine the capacity value and the input padding depending on each use cases.
- Split the obtained input in to chunks of size \\(r\\).
- Apply the \\(\mathsf{Poseidon}^\pi\\) permutation to the capacity element and the first chunk: \\(I_c \oplus m_1\\).
- Add the result to the state and apply the permutation again until no more chunks are left.
- Output \\(o\\) output elements from the bitrate part of the state.