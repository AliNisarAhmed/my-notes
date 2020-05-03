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

---

# Web Application Authentication

- Usually Auth is done with 3rd party.

### Service Provider or Relying party

- The app providing the actual service, asking for authentication. (e.g. like your own app, or sharepoint, or like Drivable Platform etc)

### Identity Provider (IdP)

- The entity providing the authentication service.

## Mechanism

- Entities
  - User
  - Web App
  - IdP
- User needs to communicate with Both Web App and IdP.
- WebApp and IdP do not directly communicate with each other

## Key Tenets

- No Direct Interaction between relying Party and IdP.
- A web app may support multiple IdP and vice verca
- However, before the user has actually Authenticated with the IdP, the web app does not know which IdP they are using.
- If the web app hints to the user to use a particular IdP, that process is called Home ground Discovery
- The entire process is async
- The Service Provider maintains no state.

## Web Application Auth protocols

- WS-Fed
- SAML (Both a protocol and a token format)
- OpenId Connect

### WS-Fed

- User visits a website
- The website says you are not authenticated
- But here is a list of IdPs I support
- the user picks an IdP
- Provide credentials
- and then through a bunch of POST requests, the Service Provider is able to identify the user

* On Azure, if we go to App registrations, we can see two endpoints related to WS-Fed
  - Federation Metadata document
  - WD-Federation sign-on endpoint - accepts our creds and issues auth token

- The most important WS-Fed request parameter is `wa` who's value is usually `wsignin1.0`

* WS-Fed uses SAML 1.1 Tokens (not to be confused with the Sign-in Protocol named SAML, which itself uses SAML 2.0 tokens)

### SAML

- User lands on a website
- Website says, you are not authenticated.
- Please go to one of this list of IdP
- So far similar to WS-Fed, but one difference is that the login request could have originated from IdP as well, not just the user

* Important parameters
  - SAMLRequest
    - Base64 envoded XML Document
  - RelayState
    - Context
  - SigAlg
    - Signature Algorithm used to sign the request
  - Signature
    - Signature of the request

#### SAML Flows

- SP-initiated
- IdP initiated

### OpenID Connect

- By far the most commonly used

### V1 vs V2 Apps

- V1 are the older OAuth apps
- V2 use OpenID Connect

### Tokens in OpenID Connect

- id_token
  - User's identity
- Refresh token
  - long lived token - used to get the fresh access tokens
- Access token
  - The short lived
  - put in the header `Authorization`

### Auth Code Flow

let
`authEndpoint = /oauth2/v2.0/authorize`
and
`tokenEndpoint = /oauth2/v2.0/token`

- User sends login request to Web app
- web app sends the user to authEndpoint
- User provides creds
- if Sign in is successful, the user is shown a consent dialog (important)
  - The consent dialog presents the user with the scopes that the web app is asking for, and ask for user'c consent
- If all goes well, the user is given back an auth code
- User takes 3 things: the code + client id (web app's id) + secret, and goes to the tokenEndpoint
- where the id_token, access_token and if asked, a refresh token is issued.
- user is also sent back permissions in the shape of scopes.
- now, with the id_token, a session is established between web app and User.

### Single Sign Out

- When the user signs out, they just dont need to sign out from the web app
- indeed, they also need to sign out of the IdP and any ADFS (Active Directory Federation Services)
- So we need to make sure we perform logout from all of these.

* So, the user logs out of the application by logging out of IdP
  - That's why there is usually a logout redirect url, which the IdP will call after a successful logout to redirect the user back to the app.

## Authentication in Web APIs

- Auth in Web APIs uses Access tokens
- These tokens are sent in the `Authorization`: `Bearer <token>` header
- The token can be opaque (not to be decoded by the client) or JWT (decodable by the client)
- These tokens are generally short lived (usually 1 hour or so)

### Validation of Auth Token

- JWK Keys - An OpenID Connect compliant server will have a well known "configuration" endpoint from where JWK keys can be downloaded. The web app is expected to cache these keys.
- Next step is to decode tokens.
- Verify the signature of the JWT
- Verify the claims in the tokens

## Scopes & Consent

- The process of exposing and granting permission is scopes and consent.
- when an API defines what an App wants to do with user can do e.g. Read and Write.

* Consent is allowing the app, by the user or the admin, the use of a certain scope

### Delegated vs Application Permission

- Delegated permission: Your application needs to access the API as the signed-in user.
- Your application runs as a background service or daemon without a signed-in user

### user_impersonation scope

- specific to azure ad
- allows the application to access a protected resource on behalf of a user
