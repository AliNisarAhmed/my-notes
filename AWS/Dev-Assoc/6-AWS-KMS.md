# AWS Key Management Services

A manage service that allows us to create, rotate, enable and disable encryption keys.

Whenever you think of data encryption in AWS, think of AWS KMS

- CMK - Customer Master key
    - serves as master/root key
    - can encrypt data up to 4kb
    - use envelope encryption for >4kb
    - 3 types
        - Customer managed CMK
            - charged monthly
        - AWS managed
            - free
        - AWS owned

- KMS supports symmetric and asymmetric
    - Symmetric - 256 bit AES
    - Asymmetric - Public/Private

## KMS API Commands

- Encrypt
    - uses CMK
    - for data < 4kb
- Decrypt
    - uses CMK
- GenerateDataKey
    - generates a data key and returns the plaintext and ciphertext version of it
- GenerateDataKeyWithoutPlaintext
    - return only ciphertext version