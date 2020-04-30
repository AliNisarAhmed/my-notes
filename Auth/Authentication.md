# Modern Authentication

- Landscape of Identity can be groupded into two parts
  - Legacy
    - Basic: Username and password
      - not secure
    - NTLM (NT LAN Manager): a challenge-response authentication process which uses three messages to authenticate a client.
    - Kerberos: a computer-network authentication protocol that works on the basis of tickets to allow nodes communicating over a non-secure network to prove their identity to one another in a secure manner.
  - Modern
    - Needed because none of the below scenarios can catered by legacy auths
      - we need to support multiple organizations working together.
      - Disparate Operating Systems and devices
      - Firewalls (maybe a hinderance to work)
      - Not real time communications
    - examples
      - WS-\* or WS-Federation
      - SAML
        - Security Assertion Markup Language
        - is an open standard for exchanging authentication and authorization data between parties, in particular, between an identity provider and a service provider. SAML is an XML-based markup language for security assertions (statements that service providers use to make access-control decisions)
      - OAuth
        - A standard for Delegation
        - Not an Auth protocol per se, but Pseudo-Authentication
          - Says something like: I am allowing "A" to do "B" on my behalf
          - But at no point does A get my password
          - "B" is a set of permissions
          - Example
            - I (Ali on twitter) am allowing
            - Some webapp A like www.abcxyz.com to
            - read my profile
            - In exchange A gets to know who I am, and offers me some functionality.
      - OpenID Connect
        - Not a panacea, but resolves some of the problems with oAuth
        - However, to implement it reliably is still the responsibility of Identity providers and web apps.
        - Adds strict standards so OAuth can be used as an authentication protocol.
        - Identity layers built on top of OAuth 2.0
        - Microsofts OpenID Connect library is MSAL (Microsoft Authentication Library)

### Problems with OAuth

- Loosly defined Standard.
  - e.g. the permission that the webapp asks for, like, for example, a webapp, to leave a comment there, is asking to know my friends like on fb, why?
- Oversharing
  - Pioneered by social media.
  - the o auth apps, in return of little, ask for just too much info sometime
  - e.g. why does candy crush need to see my photo on fb?
- Phishing attacks
  - e.g. in 2017, a million users of gmail were a target of phishing attack where the attackers seemed to share aa document from someone the person knew.
  - Since o-auth does not need a password, whoever clicked the link, their information was accessed by the attackers.

### OpenId Connect Flows

- Implicit Flows
  - Used for JS apps
- PKCE (pronounced pixi)
  - not supported with JS Apps
  - Proof key for code exchange
- AuthCode flow
  - based on tokens
  - like Id tokens, access tokens, refresh tokens
- Client Credentials flow
  - Client id (unique identifier) and a secret, get an access token back

## Federation

- The technologies, standards and use-cases which server to enable the portability of identity information across otherwise autonomous security domains.
  - Lets say a user signs into Azure AD, and we use Azure AD Connect to sync on-premises Identities (Identities local to a company's network, for example)
  - In that case, Azure AD will "federate" to an ADFS Server (Azure Directory Federation Service).
  - the real auth responsibility then falls to the ADFS, which will talk to the on-premises AD.
- For the most part, federation is not needed as a dev
  - but its good to know
  - for example, someone might say, why does Azure AD applications insists on hosting a browser window? because if the app decides to federate the identity to ADFS, then the redirect should work (native apps cant be redirected to).
