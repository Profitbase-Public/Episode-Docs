# AES Encrypt

Encrypts content using the [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algorithm.

![AES Encrypt](../../../../images/flow/security-aes-encrypt.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow encrypts an original message using the AES Encrypt action, decrypts it again using the [AES Decrypt](aes-decrypt.md) action, and then uses the [Log](../built-in/log.md) action to log the original message.

<br/>

## Properties

| Name                   | Required | Description                                                                                                                       |
| ---------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Title                  | No       | The title of the action.                                                                                                          |
| Encryption key         | Yes      | The key used to encrypt the content. This is required by the AES algorithm. The length of the key is 256 bit.                     |
| Initialization vector  | Yes      | The vector used to encrypt the content.                                                                                           |
| Input content          | Yes      | The content to be encrypted (string or byte array).                                                                               |
| Output variable name   | Yes      | The name of the variable in which the result will be stored.                                                                      |
| Output data type       | Yes      | The data type of the output variable. This can be either a string (Base64 encoded) or a byte array.                               |
| Description            | No       | Additional notes or comments about the action or configuration.                                                                   |

<br/>

## Returns

This action returns the encrypted content as a Base64 encoded string or a byte array, depending on the selected output data type.
