# AWS Cloud Security

> Hands-on with the core AWS security building blocks: identity (IAM users, roles and policies), an EC2
> instance reached with SSH keys and given an IAM role, customer-managed encryption keys (KMS), and
> encryption at rest for S3 and EBS.

*CNIT 271 (Cybersecurity Fundamentals II), Lab 3.*

## Lab setup
- AWS Management Console (lab account; reset the initial password on first login)
- IAM, EC2 (Ubuntu), KMS, S3, EBS
- SSH key pair for instance access

## Procedure

### Part 1: Identity and access management
1. Reviewed the **IAM dashboard**: account ID, resource counts, security recommendations, and the
   Users, Roles, Policies and Account settings sections.
2. **Created three IAM users**, each with its own permission policies attached and tags as key-value
   metadata. Reviewed each on the *Review and create* page before saving.
3. **Created a role** with **AWS service → EC2** as the trusted entity, attached permissions, and
   confirmed the role and its policy summary.
4. Read the JSON of the managed policy **`AmazonEC2ReadOnlyAccess`** to see how permissions are written.

**How an IAM policy is structured**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "Identifier",
    "Effect": "Allow | Deny",
    "Principal": "who (for resource-based policies)",
    "Action": "service:ApiCall",
    "Resource": "arn:aws:...",
    "Condition": { "when it applies": "..." }
  }]
}
```

### Part 2: Secure EC2 access
1. Launched an **Ubuntu EC2 instance** and created an **SSH key pair** at launch. The security group
   allowed SSH (22) for administration.
2. Created a new IAM role (**`IoTRole`**) and **attached it to the instance**, so applications on the
   instance get short-lived credentials from the role instead of stored access keys.
3. SSH'd into the instance with the private key over its public IP, checked the instance's identity
   from the shell, then **stopped the instance** to avoid leaving it running.

### Part 3: Encryption at rest with KMS
1. Created a **customer-managed KMS key** with a name and description, then set two separate permission lists:
   - **Key administrators**: who can manage or **delete** the key
   - **Key users**: who can **use** the key to encrypt and decrypt
2. Created an **S3 bucket** with default encryption **SSE-KMS** using that key.
3. Created an **EBS volume** with encryption on, using the same key.
4. **Deleted all resources** at the end. Lab or not, unused cloud resources are cost and attack surface.

## Key concepts

| Question | Answer |
| --- | --- |
| Roles vs policies | A **policy** is a JSON document of permissions and does nothing until attached. A **role** is an identity that holds policies and is *assumed* for temporary credentials, with no long-term secrets to share. |
| Which access-control model does AWS use? | **Both RBAC and ABAC.** Policies attached to roles give role-based access, and tags/conditions add attribute-based control. That avoids role explosion while keeping fine-grained control. |
| What are tags? | Key-value metadata on users and resources, used for organization, cost tracking and **ABAC conditions**. |
| What are access keys? | An access key ID and secret for programmatic access. **Never share or commit them.** Prefer roles. |

## Security takeaways
- **Separation of duties on keys:** the people who can delete a KMS key don't have to be the people who use it.
- **Roles on instances instead of hard-coded keys.** Leaked access keys are one of the most common causes of cloud breaches.
- **Encryption at rest with a key you control** means S3 and EBS data is unreadable without KMS permission.
- **Clean up resources.** Forgotten instances and volumes cost money and are attack surface.

## Skills demonstrated

AWS IAM (users, roles, policies, tags) · least privilege · RBAC/ABAC · EC2 · SSH key authentication ·
instance roles · KMS key policies · S3 SSE-KMS · EBS encryption

## Screenshots

See [`screenshots/`](screenshots/) for user, role and policy creation, the policy JSON, the EC2 launch and
SSH session, the KMS key permissions, and the encrypted S3 bucket and EBS volume.
