# Identity & Access Management

### Design Goals

- Single-Sign On (login once to multiple applications)
- Identity provider
- Reinforce Zero Trust using OIDC and OAuth2.0
- Social Login

### Too long; didn't read

**OAuth2.0** is a standard that provides secure delegated access. That means an application can take actions or access resources from a server on behalf of the user, without them having to share their credentials. It does this by allowing the identity provider (IdP) to issue tokens to third-party applications with the user’s approval.
**OpenID Connect (OIDC)** is an open standard that organizations use to authenticate users. IdPs use this so that users can sign in to the IdP, and then access other websites and apps without having to log in or share their sign-in information.
OpenID Connect is built on the OAuth2.0 protocol and uses an additional JSON Web Token (JWT), called an ID token, to standardize areas that OAuth2.0 leaves up to choice, such as scopes and endpoint discovery. It is specifically focused on user authentication and is widely used to enable user logins on consumer websites and mobile apps.

**Key Points**

- OAuth2.0 is an authorization framework that handles delegated scenarios. It's not about authentication.
- OIDC is a protocol for operating a third-party identity provider (IdP) on top of OAuth2.0. OIDC is not the IdP itself.
- We are choosing OIDC as the single sign-on (SSO) method (Because SAML was not designed for modern application types, such as SPAs and mobile apps).

### Introduction

**Single Sign-On (SSO)**
Single Sign-On (SSO) is an authentication method that lets users access multiple applications and services using a single set of login credentials. For the most part, SSOs and IdPs are separate. An SSO service uses an IdP to check user identity, but it does not actually store user identity.

**SAML**
SAML is a protocol that lets an identity provider (IdP) transmit a user's credentials to a service provider (SP) to both authenticate and authorize that user to access a service. SAML simplifies password management and enables SSO. It is mainly used for Enterprise and Government applications.

**Entity vs. Identity**
Entity refers to a thing that exists as an individual unit while identity is a set of attributes that you can use to distinguish this entity within a context.
An entity can have multiple identities.

**Authentication vs. Authorization**
Authentication is the process of confirming the identity of an entity (e.g., a user). Authorization refers to the process of verifying what entities can access, or what actions they
can perform.
An entity can use its identity to gain authorization to perform some action, but that the other way around is not possible (i.e., having authorization doesn‘t mean being authenticated or identified).

**Resource owner**
An entity capable of granting access to a protected resource. When the resource owner is a person, it is referred to as an end user.

**Resource server**
The server that hosts the protected resources, and accepts and responds to protected resource requests by using access tokens.

**Client**
An application that makes protected resource requests on behalf of the resource owner and with its authorization.

**Authorization server**
The server that issues access tokens to the client after authenticating the resource owner and obtaining authorization.

**OAuth2.0**
The OAuth2.0 authorization framework enables a third-party application (Clients) to obtain limited access to an resource server (HTTP service/API), either on behalf of a resource owner (User) by orchestrating an approval interaction between the resource owner and the resource server (HTTP service/API), or by allowing the third-party application to obtain access on its own behalf.

The important point OAuth makes is that we should never trust any application with unrestricted authentication factors (such as a password). We can never be certain the application will use these keys for our intended purpose. Instead, we should only trust applications with tokens having limited capability and a short lifespan.
OAuth was created specifically to handle delegated authorization scenarios, like when you let a random application post something on Facebook as if it was you. However, it got so much traction that developers started using it to do things it was not created to do, like handling end-user authentication, which resulted in security problems like the Confused Deputy attack. This led to an effort that resulted in _**OpenID Connect, a protocol that extends OAuth2.0**_ to address authentication.

In order to grant an application the permission to do an action (access a resource) on your behalf you are redirected to the authorization server, you perform some action there, and then the application gets an artifact related to you. The biggest difference is that, in a pure OAuth 2.0 scenario, the result will be an artifact that grants delegated authorization instead of an artifact that contains personal attributes about you.

**OpenID Connect**
OICD is a protocol that enables different types of applications to support authentication and identity management in a secure, centralized, and standardized way. Apps based on the OpenID Connect protocol rely on identity providers to handle authentication processes for them securely and to verify the identities (i.e., personal attributes) of their users.
_**OpenID Connect doesn‘t employ identity provider in it.**_ Instead, the protocol uses Authorization Server to refer to the entity in charge of authenticating end-users.
By using OpenID Connect, instead of dealing with the credentials of these users, your app would offload the authentication process to an identity provider (for example, Google, Microsoft, or Auth0). _**How these identity providers handle the authentication process is out of the scope of OpenID Connect**_. That is, from the perspective of the protocol, identity providers are free to decide if they handle user authentication through a set of credentials (e.g., username and password), if they enhance the security of the process by using features like multi-factor authentication, or even if they relay this process to other identity providers and other protocols. What OIDC defines is how identity providers and applications interact to establish end-user authentication in a secure way.
OpenID Connect take advantage of digitally signed tokens to carry enduser personal attributes around. These digital signatures enable third-party applications to confirm the veracity of the information on the fly (i.e., without having to issue yet another request to the identity provider to check the data).

### When should OpenID Connect be used

OpenID Connect can be used to enable your users to reuse their accounts on an identity provider. Instead of asking users to create yet another account, you could take advantage of OIDC to integrate with an identity provider like Google or Microsoft to let them reuse existing accounts.
Another scenario where OpenID Connect can be useful is when the protocol is used to create a hub of identity providers. In this scenario, instead of making your application communicate with multiple providers, you can make it connect to a single one that works as a hub for the others. It is much easier to support a single identity provider that acts as a hub than to support each one of them separately.
The third scenario where OpenID Connect can be really helpful is where it works as a proxy for other protocols. For example, you can make an OpenID Connect identity provider work as a proxy for a more restrictive protocol like SAML. The beauty here is that, by using this approach, you can make a resourceconstrained device integrate with a SAML identity provider through OpenID Connect.

### Discussions

Q: Why would one care about an authentication protocol?
A: because we want to solve identity management in a secure and interoperable way.

Q: Why not implement our own solution?
A: Building your own solution is hard, time consuming and not nearly as good.

Q: Why not AuthO?
A: Save money in the long run. Auth0 starts at 23 USD/month for 1.000 users.

### Implementations

- [Hydra](https://github.com/ory/hydra)

  - Open source and free
  - 10.8k Stars
  - GoLang
  - Rich Ecosystem:
    - [ORY Kratos](https://github.com/ory/kratos): Identity and User Infrastructure and Management
      - **Self-service Login and Registration**: Allow end-users to create and sign into accounts (we call them identities) using Username / Email and password combinations, Social Sign In ("Sign in with Google, GitHub"), Passwordless flows, and others.
      - **Multi-Factor Authentication (MFA/2FA)**: Support protocols such as TOTP (RFC 6238 and IETF RFC 4226 - better known as Google Authenticator)
      - **Account Verification**: Verify that an E-Mail address, phone number, or physical address actually belong to that identity.
      - **Account Recovery**: Recover access using "Forgot Password" flows, Security Codes (in case of MFA device loss), and others.
      - **Profile and Account Management**: Update passwords, personal details, email addresses, linked social profiles using secure flows.
      - **Admin APIs**: Import, update, delete identities.
    - [ORY Hydra](https://github.com/ory/hydra): OAuth2 & OpenID Connect Server
    - [ORY Oathkeeper](https://github.com/ory/oathkeeper): Identity & Access Proxy
      - ORY Oathkeeper is a BeyondCorp/Zero Trust Identity & Access Proxy (IAP) with configurable authentication, authorization, and request mutation rules for your web services: Authenticate JWT, Access Tokens, API Keys, mTLS; Check if the contained subject is allowed to perform the request; Encode resulting content into custom headers (X-User-ID), JSON Web Tokens and more!
      - This part needs to be documented :warning:
    - [ORY Keto](https://github.com/ory/keto): Access Control Policies as a Server
      - Ory Keto implements the basic API contracts for managing and checking relations ("permissions") with HTTP and gRPC APIs
      - This part needs to be documented :warning:

- [IdentityServer4](https://github.com/IdentityServer/IdentityServer4)

  - Open source and free
  - Framework for ASP.NET Core
  - 7.9k Stars
  - C#

- [SuperTokens](https://github.com/supertokens/supertokens-core)

  - Open source and free (self-hosted)
  - 2.1k Stars
  - Java
  - Login/signup customization in React

- [oidc-provider](https://github.com/panva/node-oidc-provider)

  - Open source and free
  - 1.6k Stars
  - Framework for Node.JS
  - JavaScript

- [Keycloak](https://www.keycloak.org/)

  - Free (for self-hosted)
  - Java
  - Headless (We have control over login/signup fronte-nd)
  - User Federation and Identity Brokering (OpenID Connect, OAuth2.0 and SAML 2.0)
  - Social Login (like Facebook, Twitter, LinkedIn, Instagram, GitHub, GitLab)
  - Extensible
  - E-Mail verification is built-in
  - 2FA is built-in

### Client SDKs for communicating with OAuth 2.0 and OpenID Connect providers

- [AppAuth-JS](https://github.com/openid/AppAuth-JS)
- [angular-oauth2-oidc](https://github.com/manfredsteyer/angular-oauth2-oidc)
- [angular-auth-oidc-client](https://github.com/damienbod/angular-auth-oidc-client)

### Temporary Resources:

- [How to deploy a free Auth0 alternative to DigitalOcean in 5 minutes](https://dev.to/tillsanders/how-to-deploy-a-free-auth0-alternative-to-digitalocean-in-5-minutes-2ili)
- [What is the difference between SP- and IdP-Initiated SSO?](https://support.procore.com/faq/what-is-the-difference-between-sp-and-idp-initiated-sso)
- [Identity Provider-Initiated Single Sign-On](https://auth0.com/docs/protocols/saml-protocol/saml-configuration-options/identity-provider-initiated-single-sign-on)
- [SP vs. IdP Initiated SSO](https://blogs.oracle.com/dcarru/sp-vs-idp-initiated-sso)
- [Authenticating Using SAML](https://help.liferay.com/hc/en-us/articles/360028711032-Authenticating-Using-SAML)
- [Understanding SAML](https://developer.okta.com/docs/concepts/saml/)
- [OpenID Connect Protocol](https://auth0.com/docs/protocols/openid-connect-protocol)
- [JWT Handbook](https://assets.ctfassets.net/2ntc334xpx65/o5J4X472PQUI4ai6cAcqg/13a2611de03b2c8edbd09c3ca14ae86b/jwt-handbook-v0_14_1.pdf)
- [OAuth2 and OpenID Connect: The Professional Guide - Beta](https://assets.ctfassets.net/2ntc334xpx65/5lRwNEEVa0LOAdigZ2ttYS/e804fc832c15c2633c6e1cdcfa022bd7/OAuth2_and_OpenID_Connect_The_Professional_Guide_Ebook_v02.pdf)
- [Creating An OpenID Connect System With Angular 8 And IdentityServer4 (OIDC Part 1)](https://christianlydemann.com/creating-an-openid-connect-system-with-angular-8-and-identityserver4-oidc-part-1/)

### References & Bibliography:

- [Securing Microservice APIs Sustainable and Scalable Access Control](https://docs.broadcom.com/doc/securing-microservice-apis-sustainable-and-scalable-access-control)
- [What’s the Difference Between OAuth, OpenID Connect, and SAML?](https://www.okta.com/identity-101/whats-the-difference-between-oauth-openid-connect-and-saml/)
- [The OpenID Connect Handbook, Auth0](https://assets.ctfassets.net/2ntc334xpx65/7pXoFWwEciDBmxEklweBKL/2621fe2b79c932fcddecb711bd6679fd/the-openid-connect-handbook-1_1.pdf)
- [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/id/draft-ietf-oauth-v2-31.html)
