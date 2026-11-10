# AWS Cloud Security

> Hands-on with the core AWS security building blocks: identity (IAM), encryption key management
> (KMS), and encryption at rest for S3 and EBS — plus an EC2 instance locked down with an IAM role
> and SSH key auth.

*CNIT 271 — Cybersecurity Fundamentals II, Lab 3.*

## Summary

I configured identity and access management in AWS, launched and secured an EC2 Linux instance,
and applied customer-managed encryption to storage. The focus was on least-privilege access and
protecting data at rest with keys I controlled.

## Lab setup

- AWS Management Console
- IAM (users, roles, policies), EC2 (Ubuntu), KMS, S3, EBS
- SSH key pair for instance access

## What I did &amp; found

| Area | Work | Security point |
| --- | --- | --- |
| IAM users | Created users with scoped permissions and tags | Tags enable attribute-based access + organization at scale |
| IAM roles vs policies | Attached policies (JSON permission docs) to roles | Roles grant access **without sharing long-lived credentials** |
| Access model | Reviewed how AWS authorizes | AWS blends **RBAC + ABAC** — avoids role explosion while keeping granularity |
| EC2 | Launched Ubuntu, attached an IAM role, SSH'd in with a key pair | Role on the instance beats baking in access keys |
| KMS | Created a customer-managed key, scoped admin/use permissions | Separating who can *use* vs *delete* a key limits blast radius |
| S3 | Encrypted a bucket with the KMS key | Encryption at rest under a key you control |
| EBS | Created an encrypted EBS volume with the KMS key | Same protection for block storage |

**Policy anatomy I can read and write:** `Version`, `Statement` → `Sid`, `Effect`, `Principal`,
`Action`, `Resource`, `Condition`.

**Access keys:** authenticate programmatic access (access key ID + secret). They should never be
shared or committed — the same reason the companion
[log-triage-toolkit](https://github.com/JohnGurtner/log-triage-toolkit) keeps secrets in a `.env`.

## Screenshots

See [`screenshots/`](screenshots/) for the IAM dashboard, user/role/policy creation, the SSH session
into EC2, KMS key config, and the encrypted S3 bucket + EBS volume.

## Skills demonstrated

AWS IAM · least privilege · roles vs policies · RBAC/ABAC · KMS key management ·
encryption at rest (S3, EBS) · EC2 hardening · SSH key authentication
