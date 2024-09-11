# Symmetric encryption vs Asymmetric encryption
**Symmetric-key algorithms** are algorithms for cryptography that use the same cryptographic keys for both the encryption of plaintext and the decryption of ciphertext. The keys may be identical, or there may be a simple transformation to go between the two keys. The keys, in practice, represent a shared secret between two or more parties that can be used to maintain a private information link. The requirement that both parties have access to the secret key is one of the main drawbacks of symmetric-key encryption, in comparison to public-key encryption.<sup>[1](https://en.wikipedia.org/wiki/Symmetric-key_algorithm#:~:text=Symmetric%2Dkey%20algorithms,public%2Dkey%20encryption)</sup>

How encryption works in general<sup>[2](https://www.thesslstore.com/blog/symmetric-encryption-101-definition-how-it-works-when-its-used/#:~:text=how%20encryption%20works,into%20readable%20text.)</sup>:
- The sender uses an encryption key (usually a string of letters and numbers) to encrypt their message;
- The encrypted message, called ciphertext, looks like scrambled letters and can’t be read by anyone along the way;
- The recipient uses a decryption key to transform the ciphertext back into readable text.

**Asymmetric cryptography**, also known as public-key cryptography, is a process that uses a pair of related keys - one public key and one private key - to encrypt and decrypt a message and protect it from unauthorized access or use.

A public key is a cryptographic key that can be used by any person to encrypt a message so that it can only be decrypted by the intended recipient with their private key. A private key - also known as a secret key - is shared only with key's initiator.<sup>[3](https://www.techtarget.com/searchsecurity/definition/asymmetric-cryptography#:~:text=Asymmetric%20cryptography%2C%20also,with%20key%27s%20initiator.)</sup>

Asymmetric cryptography typically gets used when increased security is the priority over speed and when identity verification is required, as the latter is not something symmetric cryptography supports. Some of the most common use cases for asymmetric cryptography include:<sup>[4](https://www.keyfactor.com/blog/symmetric-vs-asymmetric-encryption/#:~:text=Asymmetric%20cryptography%20typically%20gets%20used%20when%20increased%20security%20is%20the%20priority%20over%20speed%20and%20when%20identity%20verification%20is%20required%2C%20as%20the%20latter%20is%20not%20something%20symmetric%20cryptography%20supports.%20Some%20of%20the%20most%20common%20use%20cases%20for%20asymmetric%20cryptography%20include%3A%C2%A0)</sup> 
- **Digital signatures**: Confirming identity for someone to sign a document;
- **Blockchain**: Confirming identity to authorize transactions for cryptocurrency;
- **Public key infrastructure (PKI)**: Governing encryption keys through the issuance and management of digital certificates.

Difference Between Symmetric and Asymmetric Key Encryption:<sup>[5](https://www.geeksforgeeks.org/difference-between-symmetric-and-asymmetric-key-encryption/#:~:text=the%20service%E2%80%99s%20users.-,Difference%20Between%20Symmetric%20and%20Asymmetric%20Key%20Encryption,-Symmetric%20Key%20Encryption)</sup>

| Symmetric Key Encryption | Asymmetric Key Encryption |
|---|---|
| It only requires a single key for both encryption and decryption	| It requires two keys, a public key and a private key, one to encrypt and the other to decrypt  |
| The size of ciphertext is the same or smaller than the original plaintext	|  The size of ciphertext is the same or larger than the original plaintext  |
| The encryption process is very fast	| The encryption process is slow  |
| It is used when a large amount of data needs to be transferred	| It is used to transfer small amount of data  |
| It only provides confidentiality	| It provides confidentiality, authenticity, and non-repudiation  |
| The length of key used is 128 or 256 bits	| The  length of key used is 2048 or higher  |
| Security is lower as only one key is used for both encryption and decryption purposes	| Security is higher as two keys are used, one for encryption and the other for decryption  |
| **Examples**: 3DES, AES, DES and RC4	| **Examples**: Diffie-Hellman, ECC, El Gamal, DSA and RSA  |

# Links
[Symmetric-key algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)

[Symmetric Encryption 101: Definition, How It Works & When It’s Used](https://www.thesslstore.com/blog/symmetric-encryption-101-definition-how-it-works-when-its-used/)

[asymmetric cryptography](https://www.techtarget.com/searchsecurity/definition/asymmetric-cryptography)

[When to Use Symmetric Encryption vs. Asymmetric Encryption](https://www.keyfactor.com/blog/symmetric-vs-asymmetric-encryption/)

[Difference Between Symmetric and Asymmetric Key Encryption](https://www.geeksforgeeks.org/difference-between-symmetric-and-asymmetric-key-encryption/)

# Further Reading
[Symmetric and Asymmetric Key Encryption – Explained in Plain English](https://www.freecodecamp.org/news/encryption-explained-in-plain-english/)

[When Should You Use Symmetric vs Asymmetric Encryption?](https://venafi.com/blog/what-are-best-use-cases-symmetric-vs-asymmetric-encryption/)

# Next questions
[What do you know about Symmetric Key Encryption](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20do%20you%20know%20about%20Symmetric%20Key%20Encryption.md)

[What do you know about Asymmetric Key Encryption](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20do%20you%20know%20about%20asymmetric%20key%20encryption.md)
