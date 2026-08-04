# CST8919 Lab 3 – Enforcing Organizational Policies in Azure

## Student Information

- **Student:** Ilyas Zazai
- **Course:** CST8919 – DevOps: Security and Compliance
- **Lab:** Enforcing Organizational Policies in the Cloud

---


## Video Demonstration


---

## Lab Summary

In this lab, I used Azure Policy to enforce security and governance requirements for MapleTech Solutions.

I created three custom policy definitions, grouped them into one policy initiative called **MapleTech Secure Foundation**, and assigned the initiative to a resource group.

The initiative checks resource deployments before Azure creates them. If a resource does not follow the assigned rules, Azure denies the deployment.

## Custom Policies

### 1. Only Approved Region

- **Definition name:** `Only-CanadaCentral`
- **Current display name:** `Only East US`
- **Effect:** `Deny`

The original lab requirement was Canada Central. However, my Azure for Students subscription restricted deployments in Canada Central and East US 2. I used East US so I could complete and demonstrate the required policy tests.

This policy denies resources deployed outside East US. Global resources are excluded because some Azure services use the `global` location.

### 2. Require ProjectName Tag

- **Definition name:** `Require-ProjectName-Tag`
- **Effect:** `Deny`

This policy requires supported resources to include a `ProjectName` tag.

If the tag is missing, Azure denies the deployment.

### 3. Deny Public IP

- **Definition name:** `Deny-Public-IP`
- **Effect:** `Deny`

This policy prevents the creation of resources with the type:

```text
Microsoft.Network/publicIPAddresses
```

## Policy Initiative

The three policies were grouped into the following initiative:

```text
MapleTech Secure Foundation
```

The initiative was assigned to:

```text
rg-mapletech-policy-lab
```

The enforcement mode was enabled, so non-compliant resource deployments were denied.

## Test Results

- A virtual network in an unapproved region was denied by the location policy.
- A virtual network without the `ProjectName` tag was denied by the tagging policy.
- A Public IP Address was denied by the Public IP policy.
- A virtual network in East US with the required tag was allowed.

## Challenges and Lessons Learned

The first challenge was that the CloudLabs subscription did not allow me to create policy definitions, policy initiatives, or policy assignments. I confirmed that the required Azure Policy write permissions were excluded from the custom role, so I changed to my Azure for Students subscription.

The second challenge was that the Azure for Students subscription restricted some Azure regions. Because Canada Central and East US 2 were blocked, I used East US to complete the policy tests.

I also encountered a hidden carriage-return character in an Azure resource ID stored in a WSL variable. Azure treated the hidden character as part of the policy initiative name. I removed it using `tr -d '\r'`.

From this lab, I learned that:

- A policy definition describes one rule.
- A policy initiative groups multiple policy definitions.
- A policy assignment applies a policy or initiative to a scope.
- Creating a policy definition alone does not enforce it.
- Azure RBAC controls who can perform actions, while Azure Policy controls which resource configurations are allowed.

## Repository Structure

```text
policy-lab/
├── policy-definitions/
│   ├── only-canada-central.json
│   ├── require-project-name-tag.json
│   └── deny-public-ip.json
├── initiative/
│   └── mapletech-secure-foundation.json
├── screenshots/
└── README.md
```

