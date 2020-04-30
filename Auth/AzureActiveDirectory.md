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
