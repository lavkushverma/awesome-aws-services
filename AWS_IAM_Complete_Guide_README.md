# 🔐 AWS Identity and Access Management (IAM) -- Complete Guide

A practical, step-by-step guide to mastering IAM in Amazon Web Services.

------------------------------------------------------------------------

# 📌 What is IAM?

IAM (Identity and Access Management) is a global AWS service that helps
you securely control access to AWS resources.

With IAM, you can:

-   Create users and groups
-   Assign granular permissions using policies
-   Enable secure Console and CLI access
-   Configure Multi-Factor Authentication (MFA)
-   Create roles for AWS services like EC2, Lambda, etc.

------------------------------------------------------------------------

# 🏗 IAM Core Components

  Component           Description
  ------------------- ---------------------------------------------------
  User                Represents a person or application
  Group               Collection of users with shared permissions
  Policy              JSON document defining permissions
  Role                Temporary credentials assumed by trusted entities
  MFA                 Extra layer of authentication security
  Identity Provider   External authentication (SSO, AD, etc.)

------------------------------------------------------------------------

# 👤 IAM Users -- Detailed Explanation & Steps

## What is an IAM User?

An IAM User represents an individual person or application that needs
access to AWS.

Each user can have: - Console password - Access keys (CLI/API) -
Attached policies - MFA device

## Steps to Create an IAM User

1.  Login to AWS Console
2.  Go to IAM
3.  Click **Users** → **Create user**
4.  Enter username
5.  Choose access type:
    -   Console access
    -   Programmatic access
6.  Attach permissions (Directly / Group / Copy from user)
7.  Review and create
8.  Download credentials

------------------------------------------------------------------------

# 👥 IAM Groups -- Detailed Explanation & Steps

## What is an IAM Group?

A Group is a collection of IAM users. Permissions are assigned to the
group instead of individual users.

## Steps to Create a Group

1.  IAM → Groups
2.  Click **Create group**
3.  Enter group name (e.g., DevOps-Team)
4.  Attach required policies
5.  Add users to group
6.  Create group

------------------------------------------------------------------------

# ⚙️ IAM Settings (Account-Level Security Settings)

## Password Policy

1.  IAM → Account settings
2.  Edit password policy
3.  Configure requirements
4.  Save changes

## Account Alias

1.  IAM Dashboard
2.  Create account alias
3.  Save

------------------------------------------------------------------------

# 🏢 AWS Account -- IAM Relationship

An AWS Account is the root container for all AWS resources.

Each account contains: - Root user - IAM users - IAM roles - Policies -
Resources (EC2, S3, RDS, etc.)

### Root User Best Practices

-   Enable MFA immediately
-   Do not use for daily work
-   Create IAM admin user instead

------------------------------------------------------------------------

# 🔐 Permission Sets (IAM Identity Center)

## What is a Permission Set?

A Permission Set is a template of permissions used in AWS IAM Identity
Center (formerly AWS SSO).

## Steps to Create Permission Set

1.  Open IAM Identity Center
2.  Go to **Permission sets**
3.  Click **Create permission set**
4.  Choose predefined or custom policy
5.  Configure session duration
6.  Review and create

## Assign Permission Set

1.  Go to AWS Accounts
2.  Select account
3.  Assign users or groups
4.  Select permission set
5.  Confirm assignment

------------------------------------------------------------------------

# 🛡 IAM Best Practices

-   Never use root account for daily work
-   Enable MFA for root account
-   Follow least privilege principle
-   Use roles instead of long-term access keys
-   Rotate access keys regularly
-   Use permission boundaries for delegation

------------------------------------------------------------------------

🚀 Complete IAM guide from beginner to enterprise level.
