# AWS Organizations

This repository contains information and resources related to AWS Organizations utilising [org-formation](https://github.com/org-formation/org-formation-cli).

## Overview

AWS Organizations is a service that helps you centrally manage and govern your environment as you grow and scale your AWS resources. With Organizations, you can create accounts, organize them in hierarchies, apply policies, and automate account creation and management.

## Features

- **Centralized Management**: Manage multiple AWS accounts centrally.
- **Policy-Based Control**: Apply policies to accounts or groups of accounts.
- **Automated Account Creation**: Automate the creation and management of AWS accounts.
- **Consolidated Billing**: Combine billing for multiple accounts.
- **Automated Account Management**: When `organization.yml` is pushed master a workflow pipeline will automate the deployment.

## Prerequisite
1. Install (`aws-cli`)[https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html]
2. Create an administrator account (don't use your root account) 
   - Grant `AdministratorAccess` Policy
3. Setup aws credentials `~/.aws/credentials` in your `aws-cli` for the administrator account.

## Quick Start Guide

To get started with AWS Organizations utilising `org-formation`, follow these steps:

1. **Create an Organization**: Set up your organization in the `aws-cli` (This can also be performed via the AWS console).
    - run `aws organizations create-organization --feature-set ALL`
2. **List Accounts**: Check the structure of existing accounts.
    - `aws organizations list-roots`
    - `aws organizations list-accounts`
3. **Initialise org-formation**: 
    - `cd aws-organizations && org-formation init organization.yml`
4. **Validate Accounts**: You may need to check the verification email sent by AWS to the email address indicated as your management account this can be found in the `organization.yml` file.
5. **Organize and Add Accounts in OU (Organizationl Unit)**: Add existing accounts or create new ones. 
    - In your code editor include additional accounts 
    - Great resource to get the (org-formation grips)[https://dev.to/oconijn/off-to-a-great-start-with-aws-organizations-1i74] started.
    ```
    AWSTemplateFormatVersion: '2010-09-09-OC'
Description: default template generated for organization with master account 897722709367

Organization:
  ManagementAccount:
    Type: OC::ORG::MasterAccount
    Properties:
      AccountName: ganbatte
      AccountId: '897722709367'
      RootEmail: ${{ secrets.ROOT_EMAIL}}

  OrganizationRoot:
    Type: OC::ORG::OrganizationRoot
    Properties:
      DefaultOrganizationAccessRoleName: OrganizationAccountAccessRole

  ProductionOU:
    Type: OC::ORG::OrganizationalUnit
    Properties:
      OrganizationalUnitName: production
      Accounts: 
        - Ref: ProductionAccount1

  TestOU:
     Type: OC::ORG::OrganizationalUnit
     Properties:
      OrganizationalUnitName: test
      Accounts: 
        - Ref: TestAccount1

  TestAccount1:
    Type: OC::ORG::Account
    Properties:
      Alias: test-account-1-5577CC
      AccountName: Test Account 1
      RootEmail: smash.daybreak112@passinbox.com
  
  ProductionAccount1:
    Type: OC::ORG::Account
    Properties:
      Alias: prod-account-1-5577CC
      AccountName: Production Account 1
      RootEmail: koriyo.schnapps525@passinbox.com
    ```
5. **Apply Policies**: Use Service Control Policies (SCPs) to manage permissions.

## Documentation

- For detailed documentation, visit the [AWS Organizations Documentation](https://docs.aws.amazon.com/organizations/).
- Awesome org-formation AWS Blog (https://aws.amazon.com/blogs/opensource/managing-aws-organizations-using-the-open-source-org-formation-tool-part-1/)