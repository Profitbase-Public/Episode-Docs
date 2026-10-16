# AES Decrypt

Decrypts content using the [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algorithm.

![AES Decrypt](../../../../images/flow/security-aes-decrypt.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow encrypts an original message using the [AES Encrypt](aes-encrypt.md) action, decrypts it again using the AES Decrypt action, and then uses the [Log](../built-in/log.md) action to log the original message.

<br/>

## Properties

| Name                   | Required | Description                                                                                                                       |
| ---------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Title                  | No       | The title of the action.                                                                                                          |
| Encryption key         | Yes      | The key used to decrypt the content. This must be the same key that was used to encrypt the content.                              |
| Initialization vector  | Yes      | The vector used to decrypt the content. This must be the same vector that was used to encrypt the content.                        |
| Input content          | Yes      | The content to be decrypted (string or byte array).                                                                               |
| Output variable name   | Yes      | The name of the variable in which the result will be stored.                                                                      |
| Output data type       | Yes      | The data type of the output variable. This can be either a string (Base64 encoded) or a byte array.                               |
| Description            | No       | Additional notes or comments about the action or configuration.                                                                   |

<br/>

## Returns

This action returns the decrypted content as a string or a byte array, depending on the selected output data type.
