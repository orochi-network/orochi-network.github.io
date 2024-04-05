# Poseidon\\(^\pi\\) for encryption

## Scenario
Let \\(B \in \mathbb{F}_q^2\\) be a curve point of prime order \\(p\\). Let \\((k,K = [k]B)\\) be a keypair with \\(k < p\\) as a ***private key***. We consider the following scenario:
1. **Alice** approaches **Bob**, who has ***public key*** \\(K\\).
2. **Alice** selects message of \\(l\\) \\(\mathbb{F}_q\\) elements long.
3. **Alice** selects ***nonce*** \\(N\\) (it can be a timestamp).
4. **Alice** generates a ***shared secret key*** \\(k_S\\) using \\(K\\).
5. **Alice** encrypts \\(M\\) using \\(N, k_S\\) with an ***authenticated encryption scheme*** \\(\varepsilon\\) and gets ***ciphertext*** \\(C\\).
6. **Alice** sends \\(C\\) and additional information \\(\mathcal{A}\\), needed to decrypt \\(C\\), to **Bob**.
7. **Bob** decrypts \\(C\\) and processes \\(M\\). The ciphertext integrity property of \\(\varepsilon\\) ensures it has not been modified in between.

## Design: Parameters

Poseidon permutation: \\(Poseidon^\pi: \mathbb{F}_q^4 \rightarrow \mathbb{F}_q^4\\)
The SBox is recommended as \\(x^5\\) for \\(q\\) being the prime subgroup size of curve BN254.

## Design: Shared key

This procedure is used by **Alice** in step \\(4\\): generates the shared secret key \\(k_S\\) using \\(K\\).

1. Generate random \\(r < p\\).
2. Generate ***expanded encryption key***:
$$k_S \leftarrow [r]K = (k_S[0], k_S[1]) \in \mathbb{F}_q^2$$

## Design: Encryption

This procedure is used by **Alice** in step \\(5\\).

Encrypt message \\(M \in \mathbb{F}_p^l\\) of length \\(l\\) with the nonce \\(N < 2^{128}\\) using shared key \\(k_S\\).

1. Create Poseidon input state $$S = (0, k_S[0], k_S[1], N + l * 2^{128}) \in \mathbb{F}_q^4$$
2. Repeat for \\(0 \leq i \lceil l/3 \rceil\\):

    a. Iterate Poseidon on \\(S\\): $$S \leftarrow \mathcal{P}(S)$$
    b. Absord three elements of message (in the last iteration the missing elements are set to \\(0\\)):
    $$S[1] \leftarrow S[1] + M[3i];$$$$S[2] \leftarrow S[2] + M[3i + 1];$$$$S[3]\leftarrow S[3] + M[3i + 2].$$ 
    c. Release three elements of ciphertext: $$C[3i] \leftarrow S[1];$$$$C[3i + 1] \leftarrow S[2];$$$$C[3i + 2] \leftarrow[3].$$
3. Iterate Poseidon on $S$ last time: $$S \leftarrow \mathcal{P}(S)$$
4. Release the last ciphertext element: $$C.add(S[1]).$$

## Design: Decryption

Bob obtains \\((C, A' = [r]B, N, l)\\) and decrypts as follows:
1. Generate \\(k_S \leftarrow [k]A'\\)
2. Create Poseidon input state $$S = (0, k_S[0], k_S[1], N + l*2^{128}).$$
3. Repeat for \\(0 \leq i \lceil l/3 \rceil\\)
    a. Iterate Poseidon on \\(S\\): $$S \leftarrow \mathcal{P}(S).$$
    b. Release three elements of message: $$M[3i] \leftarrow S[1] + C[3i];$$$$M[3i + 1] \leftarrow S[2] + C[3i+1];$$$$M[3i+2] \leftarrow S[3] + C[3i+2].$$
    c. Modify state: $$S[1] \leftarrow C[3i];$$$$S[2] \leftarrow C[3i+1];$$$$S[3] \leftarrow C[3i+2].$$
    d. *Check ciphertext integrity property*: If \\(3 \nmid l\\) then check that **the last \\(3 - (l\mod3)\\) elements of \\(M\\) are \\(0\\)**. If not, reject the ciphertext.
4. Iterate Poseidon on \\(S\\) last time: $$S \leftarrow \mathcal{P}(S);$$
5. Check the last ciphertext elements: $$C.last = S[1].$$