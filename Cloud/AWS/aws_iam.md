# IAM Concepts and Capabilities

## 1. Permissions and Policies

- **Policies**: Documents that define permissions, written in JSON format.
- **Managed Policies**: Created and managed by AWS (AWS Managed Policies) or by
  the customer (Customer Managed Policies).
- **Inline Policies**: Policies embedded directly within a user, group, or role.
- **Permission Boundaries**: Policies that define the maximum permissions an IAM
  entity can have.

## 2. IAM Users

- Users are identities used to interact with AWS.
- Users can have:
  - Directly attached policies.
  - Membership in one or more groups.
  - Credentials for API access (access keys) and/or AWS Management Console
    access (password).

## 3. IAM Groups

- Collections of IAM users.
- Policies attached to groups are inherited by all users in the group.
- Users can belong to multiple groups.

## 4. IAM Roles

- Used to grant temporary permissions to entities that can assume the role.
- Suitable for:
  - Cross-account access.
  - Granting permissions to AWS services (e.g., EC2, Lambda).
  - Temporary access needs.

## 5. Identity Providers and Federation

- **Federation**: Allows external users to access AWS resources without needing
  an IAM user.
  - Web Identity Federation (Amazon, Google, Facebook).
  - SAML 2.0-Based Federation (corporate directories, SSO).
- **Identity Providers**: Third-party services that provide authentication
  (e.g., Active Directory, Okta).

## 6. IAM Access Analyzer

- Analyzes and identifies resources shared with external entities.
- Helps ensure that resources are not unintentionally shared.

## 7. Service Control Policies (SCPs)

- Used with AWS Organizations to manage permissions across multiple accounts.
- Define guardrails for what actions accounts in the organization can perform.

## 8. Multi-Factor Authentication (MFA)

- Adds an extra layer of security by requiring a second form of authentication
  (e.g., code from a hardware token or app).

## 9. Credential Reports

- Provide a report of all users and the status of their credentials (passwords,
  access keys).
- Useful for auditing and security reviews.

## 10. Resource-Based Policies

- Policies attached directly to AWS resources (e.g., S3 buckets, SNS topics, SQS
  queues).
- Define who can access the resource and what actions they can perform.

## 11. IAM Permissions Boundaries

- Limit the maximum permissions a user or role can have.
- Useful for delegating permission management while enforcing security
  constraints.

## 12. AWS Organizations and SCPs

- **AWS Organizations**: Manage multiple AWS accounts centrally.
- **Service Control Policies (SCPs)**: Define permissions at the organizational
  unit or account level, acting as permission guardrails.

# Examples of Usage Scenarios

- **Direct User Permissions**: Attach policies directly to a user for
  fine-grained control.
- **Group Permissions**: Simplify management by attaching policies to groups and
  adding users to these groups.
- **Role-Based Access**: Use roles to grant temporary permissions or
  cross-account access.
- **Federation**: Allow external users (e.g., employees, partners) to access AWS
  resources using existing corporate credentials.
- **MFA**: Enhance security by requiring multi-factor authentication for
  sensitive operations.

# Summary of Relationships

- **Permissions -> Policies -> Users**: Directly attach policies to users to
  grant permissions.
- **Permissions -> Policies -> Groups -> Users**: Attach policies to groups, and
  users inherit these permissions by being members of the group.
- **Permissions -> Policies -> Roles -> Users**: Create roles with attached
  policies, and users assume these roles to gain permissions.
