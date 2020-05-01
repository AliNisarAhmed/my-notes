# Azure Active Directory

- AAD is modern cloud based Identity and access management service.

---

From MS website

> Azure Active Directory is an Identity and Access Management as a service (IDaaS) solution that extends your on-premises directories into the cloud and provides single sign-on to Azure, Office 365 and thousands of cloud (SaaS) apps and access to web apps you run on-premises.

> Built for ease of use, Azure Active Directory enables enterprise mobility and collaboration and delivers advanced identity protection through multi-factor authentication (MFA), security reports, audits, alerts and adaptive conditional access policies based on device health, user location and risk level.

---

- A modern cloud based must support
  - Authentication for Users, managing devices and Applications
  - Internet scale - should work across the globe. Also does not have a single point of failure
  - Secure and Resilient

## Authentication in Microsoft Ecosystem

- On-premises
  - e.g. Corboros which works with a domain controller to check access to a network
- Cloud based authentication
- Distributed Authentication

AzureAD Connect connects on-premises Auths to the cloud and makes the Idenitities available in the cloud.

AzureAD uses Standards Based Authentication, meaning someone supporting those standards, even when not using MS products, can still work with AzureAD.

## Azure AD Users, Admins and Roles

- Once we have an Azure Active Directory set up, we can create additional users.
- An Azure AD needs subscriptions in order to use portal services.
- We can assign roles to the users in our AD, for example, Global Admin
- We can also create non admin roles like User.

## App management

- SIngle Tenant Apps
- Multi Tenant apps
- Enterprise Apps (precreated ready for use Multi tenant Apps). e.g. using Dropbox in our apps, but singing in to Dropbox using Azure AD.

#### Public vs Confidential Client

- Public client: an SPA, MS WPF apps etc, which cannot be trusted to keep a "secret", because they are public client
- Confidential Client: e.g. Server where Authentication occurs, here we are confident that we can keep secrets since these apps are not exposed to the public.

## App Registrations

- We can register Enterprise apps (e.g. Docusign, github, G Suite) and manager their sign ins using Azure Ad. (These are also called Gallery applications)
  - we can also add non-gallery apps, but these must conform with the Azure AD known standards like SAML, or SCIM

## Service Principals

- In Azure, Service Principals are distinct from Apps, though the two are frequently confused
- A Service Principal is an Identity created for use with Applications, Hosted services and other things in Azure.
- The appropriate uses for Service Principals is usually Automated Scenarios, where we may not have the UI to login, so we can use Azure CLI to login using SP.
- When we create a managed identity or an app on Azure, Service Principals are being created to handle auth for those apps.
- We have the ability to add Service principals to Groups, and then assign Roles, credentials to that group.

#### Relation between Apps and Service Principals

- Apps and SPs are different entities.
- Whenever am app gets any permission, it must be associted with an SP.
- By default, when we create an App on Azure, it gets the `User.Read` permission
- When we create an app, we get an SP with it, and when we create an SP, we get an app with it. They are joined at the hip.
- It is possible to create an app without SP, but it wont have any permissions, as soon as we give it some permission, it gets assigned an SP.
- It is common to have SPs without Apps (for RBAC or managed identities) but rare to have Apps w/o SPs

---
