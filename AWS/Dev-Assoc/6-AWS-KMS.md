# AWS Key Management Services

A manage service that allows us to create, rotate, enable and disable encryption keys.

Whenever you think of data encryption in AWS, think of AWS KMS

- CMK - Customer Master key (separate from customer managed key)
    - serves as master/root key
    - can encrypt data up to 4kb
    - use envelope encryption for >4kb
        - Encrypt plaintext data with a data key and then encrypt the data key with a top-level plaintext master key
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


## Key rotation best practice

- AWS keys need not be rotated as AWS auto rotates them every year
- Enable auto rotation on CMKs to rotate keys every year
- Manually rotate Asymmetric KMS keys every year