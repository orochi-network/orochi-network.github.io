# Serialization

Serialization is the process of converting an object or data structure into a format that can be easily stored, transmitted, and reconstructed later. It is often used to save the state of a program, send data over a network, or store complex data structures, such as objects, in a human-readable or compact binary format. The opposite process, called deserialization, converts the stored format back into an object or data structure.

## Data Type

### SnaryJS supported types

- **Built-in types**
  - Field
  - Bool
  - UInt32
  - UInt64
  - PublicKey
  - PrivateKey
  - Signature
  - Group
  - Scalar
  - CircuitString
  - Character
- **Custom types**
  - Struct [\*](https://docs.minaprotocol.com/zkapps/snarkyjs-reference/modules#struct-1)
- **Trees**
  - MerkleTree
  - MerkleMap

### Bson supported types

- Double
- String
- Object
- Array
- Binary data
- Undefined
- ObjectId
- Boolean
- Date
- Null
- Regular Expression
- DBPointer
- JavaScript
- Symbol
- 32-bit integer
- Timestamp
- 64-bit integer
- Decimal128
- Min key
- Max key

## Serialization/Deserialization

The provided code snippet demonstrates how to convert a zk-snark data type into a BSON-supported format by first converting the value into a Uint8Array and then serializing it using BSON.

```ts
const value = UInt64.from(12342);
const bytes: Uint8Array = Encoding.Bijective.Fp.toBytes(value.toFields());
const bson = BSON.serialize({ bytes });
```

This code snippet demonstrates the process of converting BSON data back into a zk-SNARK data type. This is done by first deserializing the BSON data into a JavaScript object, then converting the Binary data into a Uint8Array, and finally using a built-in decoding method to reconstruct the original value from the byte array.

```ts
const deserializedBson = BSON.deserialize(bson);
const convertedResult = new Uint8Array(deserializedBson.bytes.buffer);
const initialField = Encoding.Bijective.Fp.fromBytes(convertedResult);
```

### Serializing Arbitrary Data into Field Elements

When serializing arbitrary data into field elements, it's important to note that field elements can hold a maximum of 254 arbitrary bits (not 255) due to the largest possible field element lying between 2^254 and 2^255.

You can utilize the `Encoding.bytesToFields` method, which efficiently packs 31 bytes per field element for serialization.

**HELP** We need to clarify which kind of data type will be supported.