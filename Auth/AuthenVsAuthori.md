# Authentication vs Authorization

- Authentication means checking that the user is the same person that they claim to be
- Authorization is checking if the user has access to resouces that they claim to have.

### Claims

- Claims by users are the things that the user claim they have access to, for example, in OpenId Connect, UPN, Audience, Validity. Or in general Country, IP, Payment, Group.

### Scopes

- Scope is the actions that a claim is allowed to do, so for example, a READ claim may allow users to READ all the data without ability to WRITE.
  - an APPROVE claim may allow a user to approve certain resource creation.

### Complicated Authorizations

- Examples
  - Time based Authorization (Certain users allowed to perform certain tasks only at certain times), also called Priviliged Identitty Management.
  - Offline Scenarios
    - An Identified user, who is now offline, are they allowed to do certaina actions.
  - Just in Time
    - You get an hour long valid Access token, but in the meantime, your account was disabled, so the token is now useless. To implement this logic will be JIT authorization.

### Common Approaches to Authorization

- **RBAC**: Role Based Access Control
- **Scope** based
- **Claim** based

**RBAC**

- Arrange users in groups
- define Roles for those groups
- Define what the role does using Role Definitions

**Scopes**

- How you define your API, e.g. If you have READ scope, you can READ on all resources.
- What Scopes you have access to show up in the `scp` claim
  - not just the scopes that you have access to, but also the scopes that you have consented to. (An admin may have added a claim for you on your behalf)
- Scopes is also how we request for addition scopes
  - e.g. in OpenId Connect, we can request PROFILE scope, when we do that we are given other details, like user name and profile information.

**Claims**

- Claims are attributes
  - Some claims/attributes are industry standard, but we can define our own claims.
- You can grant/deny access based on the attribute values
