# Security overview

The **Security** category contains actions for encrypting and decrypting content within a flow using the AES algorithm. Use these actions when sensitive content (such as a payload, file, or secret value) needs to be protected before being stored or transmitted, and decrypted later by a flow that holds the same key.

The actions in this category don't use a connection — they operate on in-memory content passed in as a string or byte array.

<br/>

## Explore

#### Encrypting content
[AES Encrypt](./aes-encrypt.md) encrypts a string or byte array using a 256-bit encryption key and an initialization vector. The result is returned either as a Base64-encoded string or a byte array, depending on the chosen output type.

<br/>

#### Decrypting content
[AES Decrypt](./aes-decrypt.md) reverses the operation, taking the encrypted input together with the same encryption key and initialization vector that were used during encryption, and returning the original string or byte array.
