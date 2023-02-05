# AWS S3

Object vs Bucket
- object is a file with metadata
    - metadata is kv pair
    - metadata does not change once a file is uploaded
- bucket contains many objects
    - bucket names are globally unique


S3 has a flat structure, concept of a folder is not hierarchical, rather folders are just name prefixes
- S3 does not support POSIX
    - concurrent file modification
    - FS access symantics
    - file locking


S3 auto replicates objects to all AZs of a region

S3 has 99.99% Availability and 99.999999999% Durability (11 9s)
    - durability is the probability that the object remains intact after a period of 1 year

S3 storage classes
- S3 Standard
    - For frequently accessed data
    - highly durable
- S3 Standard-IA (Infrequent Access)
- S3 One Zone-IA
- S3 Intelligent-Tiering
    - for changing or unknown access patterns
- S3 Standard-IA & S3 One Zone-IA
    - Infrequest access, long-lived data
- S3 Glacier and S3 Glacier Deep Archive
    - For low cost long term storage and data archiving

S3 supports lifecycle policy where data can move to different stoage classes based on access patterns

We can set up
- S3 Versioning and MFA
    - to prevent accidental data deletion
- Access Control List
    - Secure access to your S3 buckets and objects
- Bucket Policy
    - Control external access to S3 Bucket


Cross-Region replication
- Auto replicate objects to a different AWS Region for backup purposes