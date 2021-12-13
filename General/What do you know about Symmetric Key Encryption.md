# Symmetric Key Encryption
**Symmetric-key algorithms** are algorithms for cryptography that use the same cryptographic keys for both the encryption of plaintext and the decryption of ciphertext. The keys may be identical, or there may be a simple transformation to go between the two keys. The keys, in practice, represent a shared secret between two or more parties that can be used to maintain a private information link. The requirement that both parties have access to the secret key is one of the main drawbacks of symmetric-key encryption, in comparison to public-key encryption. 

How encryption works in general:
- The sender uses an encryption key (usually a string of letters and numbers) to encrypt their message;
- The encrypted message, called ciphertext, looks like scrambled letters and can’t be read by anyone along the way;
- The recipient uses a decryption key to transform the ciphertext back into readable text.

![](./res/symmetric_encryption.png "Symmetric encryption")

In the example above, we used the same key for encryption and decryption, which means this is symmetric encryption.

## Types
Symmetric-key encryption can use either stream ciphers or block ciphers.
- Stream ciphers encrypt the digits (typically bytes), or letters (in substitution ciphers) of a message one at a time;
- Block ciphers take a number of bits and encrypt them as a single unit, padding the plaintext so that it is a multiple of the block size. 

## What is Symmetric Encryption Used For?
While symmetric encryption is an older method of encryption, it is faster and more efficient than asymmetric encryption, which takes a toll on networks due to performance issues with data size and heavy CPU use. Due to the better performance and faster speed of symmetric encryption (compared to asymmetric), symmetric cryptography is typically used for bulk encryption / encrypting large amounts of data, e.g. for database encryption. In the case of a database, the secret key might only be available to the database itself to encrypt or decrypt.

Some examples of where symmetric cryptography is used are:
- Payment applications, such as card transactions where PII needs to be protected to prevent identity theft or fraudulent charges;
- Validations to confirm that the sender of a message is who he claims to be;
- Random number generation or hashing.

# Links
[Symmetric-key algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)

[Symmetric Key Encryption - why, where and how it’s used in banking](https://www.cryptomathic.com/news-events/blog/symmetric-key-encryption-why-where-and-how-its-used-in-banking)

[Symmetric Encryption 101: Definition, How It Works & When It’s Used](https://www.thesslstore.com/blog/symmetric-encryption-101-definition-how-it-works-when-its-used/)

# Further reading
[Symmetric vs. Asymmetric Encryption: What's the Difference?](https://www.trentonsystems.com/blog/symmetric-vs-asymmetric-encryption)