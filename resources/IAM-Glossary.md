# IAM Glossary

A comprehensive glossary of Identity and Access Management terms and concepts.

---

## A

**Access Control**: The process of granting or denying specific requests to obtain and use information and related information processing services.

**Access Control List (ACL)**: A list of permissions attached to an object specifying which users or system processes can access that object.

**Access Token**: A credential used to access protected resources, typically issued after successful authentication.

**Adaptive Authentication**: Dynamic authentication process that adjusts security requirements based on risk assessment.

**API Key**: A unique identifier used to authenticate requests to an API.

**Assertion**: A statement made by an entity without accompanying evidence of its validity (used in SAML).

**Attribute-Based Access Control (ABAC)**: Access control paradigm where access decisions are based on attributes (user, resource, environment).

**Authentication**: The process of verifying the identity of a user, device, or system.

**Authorization**: The process of determining what an authenticated entity is allowed to do.

**Authorization Code**: A temporary code issued during OAuth 2.0 flow, exchanged for an access token.

**Authorization Server**: The server issuing access tokens after successfully authenticating the resource owner.

---

## B

**Bearer Token**: An access token that can be used by anyone possessing it, without additional credentials.

**Biometric Authentication**: Authentication based on biological characteristics (fingerprint, face, iris).

**Brute Force Attack**: Attempting to guess credentials by systematically trying all possible combinations.

---

## C

**Certificate Authority (CA)**: Entity that issues digital certificates.

**Claim**: A piece of information asserted about a subject (e.g., name, email, role).

**Client**: An application making protected resource requests on behalf of the resource owner.

**Client Credentials**: Authentication method where client authenticates directly (no user involved).

**Client ID**: Public identifier for an OAuth 2.0 client application.

**Client Secret**: Confidential secret known only to the client and authorization server.

**Consent**: User's explicit permission to share information or allow actions.

**Credential Stuffing**: Attack using breached username/password pairs from other services.

---

## D

**Decentralized Identity**: Identity model where users control their own identity without central authority.

**Digital Certificate**: Electronic document proving ownership of a public key.

**Digital Signature**: Mathematical scheme for verifying authenticity and integrity of digital messages.

**Directory Service**: Shared information infrastructure for locating, managing, and organizing resources (e.g., LDAP).

---

## E

**Encryption**: Process of encoding information so only authorized parties can access it.

**Entity**: Something that has separate and distinct existence (user, device, service).

---

## F

**Federated Identity**: Linking a user's identity across multiple identity management systems.

**FIDO (Fast Identity Online)**: Set of standards for passwordless authentication.

**FIDO2**: Latest set of FIDO specifications including WebAuthn and CTAP.

---

## G

**Grant Type**: Method of obtaining an access token in OAuth 2.0.

---

## H

**Hash Function**: One-way function that converts input into fixed-size string (used for password storage).

**HOTP (HMAC-based One-Time Password)**: Algorithm generating one-time passwords based on counter.

---

## I

**Identity**: Set of attributes that uniquely describe an entity.

**Identity Federation**: Process of linking identity information across trust domains.

**Identity Provider (IdP)**: System that creates, maintains, and manages identity information.

**Identity Proofing**: Process of verifying identity before issuing credentials.

**ID Token**: JWT containing user identity claims (OpenID Connect).

**Impersonation**: Acting as another user with their permissions.

**Introspection**: Process of obtaining metadata about an access token.

---

## J

**JSON Web Key (JWK)**: JSON representation of cryptographic key.

**JSON Web Key Set (JWKS)**: Set of JWKs.

**JSON Web Signature (JWS)**: Digitally signed or MACed content using JSON.

**JSON Web Token (JWT)**: Compact, URL-safe means of representing claims between parties.

**Just-in-Time (JIT) Provisioning**: Creating user accounts at first login rather than pre-provisioning.

---

## K

**Kerberos**: Network authentication protocol using secret-key cryptography.

**Key**: Piece of information used to encrypt/decrypt data or verify signatures.

---

## L

**LDAP (Lightweight Directory Access Protocol)**: Protocol for accessing directory services.

**Least Privilege**: Security principle of granting minimum permissions necessary.

---

## M

**MFA (Multi-Factor Authentication)**: Authentication requiring two or more verification factors.

**Mutual TLS (mTLS)**: Authentication where both client and server verify each other's identity.

---

## N

**Nonce**: Random value used once to prevent replay attacks.

---

## O

**OAuth 2.0**: Authorization framework enabling third-party access without sharing credentials.

**OIDC (OpenID Connect)**: Authentication layer built on OAuth 2.0.

**One-Time Password (OTP)**: Password valid for single use or time period.

---

## P

**Password Hash**: Encrypted representation of password stored in database.

**Passwordless Authentication**: Authentication without traditional passwords.

**PBKDF2**: Password-Based Key Derivation Function 2 (password hashing algorithm).

**Phishing**: Fraudulent attempt to obtain sensitive information.

**PKCE (Proof Key for Code Exchange)**: OAuth 2.0 extension preventing authorization code interception.

**Policy**: Set of rules governing access decisions.

**Principal**: Entity whose identity can be authenticated.

**Privilege Escalation**: Gaining elevated access beyond intended permissions.

**Provisioning**: Process of creating, managing, and removing user accounts.

---

## R

**RBAC (Role-Based Access Control)**: Access control based on roles assigned to users.

**Realm**: Security policy domain in which authorization decisions are made.

**Redirect URI**: URL where user is redirected after authorization.

**Refresh Token**: Long-lived token used to obtain new access tokens.

**Registration**: Process of creating new user account.

**Resource Owner**: Entity capable of granting access to protected resource (usually end-user).

**Resource Server**: Server hosting protected resources.

**Revocation**: Invalidating previously issued token or credential.

**Risk-Based Authentication**: Authentication adjusting requirements based on calculated risk.

---

## S

**SAML (Security Assertion Markup Language)**: XML-based framework for exchanging authentication and authorization data.

**Scope**: Permission level requested/granted for access token.

**SCIM (System for Cross-domain Identity Management)**: Standard for user provisioning.

**Secret**: Confidential information used for authentication.

**Security Token**: Data representing authentication or authorization information.

**Self-Service**: Allowing users to manage their own accounts without admin intervention.

**Service Account**: Account used by application or service rather than person.

**Service Provider (SP)**: System providing service to authenticated users (SAML term).

**Session**: Period during which user is authenticated.

**Session Fixation**: Attack forcing user session ID to known value.

**Session Hijacking**: Exploiting valid session to gain unauthorized access.

**Single Sign-On (SSO)**: Authentication allowing access to multiple applications with one login.

**Social Login**: Authenticating using social media account credentials.

**Subject**: Entity about which claims are made (typically user).

---

## T

**Tenant**: Isolated group of users within multi-tenant system.

**Token**: String representing authentication/authorization information.

**Token Binding**: Cryptographically binding security tokens to TLS layer.

**Token Endpoint**: OAuth 2.0 endpoint for obtaining tokens.

**TOTP (Time-based One-Time Password)**: Algorithm generating one-time passwords using current time.

**Trust**: Relationship where one entity believes statements from another.

**Two-Factor Authentication (2FA)**: Authentication using two different factors.

---

## U

**User Agent**: Application acting on behalf of user (typically web browser).

**User Federation**: Connecting external user stores to identity system.

**Userinfo Endpoint**: OIDC endpoint returning user profile information.

---

## V

**Verification**: Confirming authenticity or validity of something.

---

## W

**WebAuthn**: Web standard for passwordless authentication.

**Well-Known URI**: Standard endpoint for discovering service configuration.

---

## X

**X.509**: Standard for public key certificates.

**XACML (eXtensible Access Control Markup Language)**: XML-based language for access control policies.

---

## Z

**Zero Trust**: Security model assuming no implicit trust, requiring continuous verification.

**Zero Trust Architecture (ZTA)**: Security architecture implementing Zero Trust principles.

---

## Acronyms Quick Reference

- **2FA**: Two-Factor Authentication
- **ABAC**: Attribute-Based Access Control
- **ACL**: Access Control List
- **API**: Application Programming Interface
- **CA**: Certificate Authority
- **CIAM**: Customer Identity and Access Management
- **FIDO**: Fast Identity Online
- **IAM**: Identity and Access Management
- **IdP**: Identity Provider
- **JIT**: Just-in-Time
- **JWKS**: JSON Web Key Set
- **JWT**: JSON Web Token
- **LDAP**: Lightweight Directory Access Protocol
- **MFA**: Multi-Factor Authentication
- **mTLS**: Mutual Transport Layer Security
- **OAuth**: Open Authorization
- **OIDC**: OpenID Connect
- **OTP**: One-Time Password
- **PAM**: Privileged Access Management
- **PBAC**: Policy-Based Access Control
- **PKCE**: Proof Key for Code Exchange
- **RBAC**: Role-Based Access Control
- **SAML**: Security Assertion Markup Language
- **SCIM**: System for Cross-domain Identity Management
- **SoD**: Separation of Duties
- **SP**: Service Provider
- **SSO**: Single Sign-On
- **TLS**: Transport Layer Security
- **TOTP**: Time-based One-Time Password
- **ZTA**: Zero Trust Architecture

---

**Note**: This glossary is continuously updated. Contributions are welcome!
