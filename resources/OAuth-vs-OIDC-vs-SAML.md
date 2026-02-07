# OAuth 2.0 vs OpenID Connect vs SAML 2.0

A comprehensive comparison of the three major authentication and authorization protocols.

---

## Overview

| Aspect | OAuth 2.0 | OpenID Connect | SAML 2.0 |
|--------|-----------|----------------|----------|
| **Primary Purpose** | Authorization | Authentication | Authentication & Authorization |
| **Format** | JSON/JWT | JSON/JWT | XML |
| **Transport** | HTTP/REST | HTTP/REST | HTTP/SOAP |
| **Token Type** | Access Token | ID Token + Access Token | SAML Assertion |
| **Best For** | API Authorization | Modern Web/Mobile Apps | Enterprise SSO |
| **Year Introduced** | 2012 | 2014 | 2005 |
| **Complexity** | Medium | Medium | High |
| **Mobile Support** | Excellent | Excellent | Poor |
| **Browser Support** | Excellent | Excellent | Good |

---

## OAuth 2.0

### What is OAuth 2.0?

OAuth 2.0 is an **authorization framework** that enables applications to obtain limited access to user accounts on an HTTP service. It works by delegating user authentication to the service hosting the user account and authorizing third-party applications to access that user account.

### Key Characteristics

- **Purpose**: Authorization (not authentication)
- **Format**: JSON-based
- **Tokens**: Access tokens, refresh tokens
- **Scopes**: Define permission levels
- **Grant Types**: Multiple flows for different scenarios

### OAuth 2.0 Grant Types

#### 1. Authorization Code Flow
```
User → Client → Auth Server → Authorization Code → Client → Access Token
```
**Use Case**: Web applications with backend
**Security**: Most secure (especially with PKCE)

#### 2. Authorization Code Flow with PKCE
```
Client generates code_verifier and code_challenge
Same as above but with additional security for public clients
```
**Use Case**: Mobile apps, SPAs, any public client
**Security**: Prevents authorization code interception

#### 3. Client Credentials Flow
```
Client → Auth Server → Access Token
```
**Use Case**: Machine-to-machine, backend services
**Security**: Client secret required

#### 4. Implicit Flow (Deprecated)
**Status**: Deprecated in OAuth 2.1
**Reason**: Security vulnerabilities

#### 5. Resource Owner Password Credentials (Deprecated)
**Status**: Not recommended, deprecated in OAuth 2.1
**Reason**: User credentials exposed to client

### OAuth 2.0 Endpoints

- **Authorization Endpoint**: Where users authenticate and authorize
- **Token Endpoint**: Where tokens are issued
- **Introspection Endpoint**: Validate token (optional)
- **Revocation Endpoint**: Revoke token (optional)

### OAuth 2.0 Tokens

#### Access Token
- Short-lived (minutes to hours)
- Bearer token
- Used to access protected resources
- Can be opaque or JWT

#### Refresh Token
- Long-lived (days to months)
- Used to obtain new access tokens
- Should be stored securely
- Optional

### Common Misconceptions

❌ **"OAuth 2.0 is for authentication"**
- OAuth 2.0 is for authorization, not authentication
- You can't reliably authenticate users with OAuth 2.0 alone

❌ **"Access token proves user identity"**
- Access token proves authorization, not identity
- Different users might have same access token format

### When to Use OAuth 2.0

✅ API authorization
✅ Delegated access scenarios
✅ Third-party app integration
✅ Microservices authorization

---

## OpenID Connect (OIDC)

### What is OpenID Connect?

OpenID Connect is an **authentication layer** built on top of OAuth 2.0. It allows clients to verify the identity of end-users and obtain basic profile information.

### Key Characteristics

- **Purpose**: Authentication (built on OAuth 2.0 authorization)
- **Format**: JSON-based, uses JWT
- **Tokens**: ID token + access token + refresh token
- **Profile**: UserInfo endpoint for user details
- **Standards**: Well-defined specification

### OIDC Components

#### ID Token (JWT)
```json
{
  "iss": "https://auth.example.com",
  "sub": "user123",
  "aud": "client-app-id",
  "exp": 1645564800,
  "iat": 1645561200,
  "nonce": "random-value",
  "email": "user@example.com",
  "email_verified": true,
  "name": "John Doe"
}
```

**Contains**:
- `iss`: Issuer (Identity Provider)
- `sub`: Subject (unique user identifier)
- `aud`: Audience (client ID)
- `exp`: Expiration time
- `iat`: Issued at time
- Custom claims (name, email, etc.)

#### Access Token
- Same as OAuth 2.0
- Used to access UserInfo endpoint
- Can access other APIs if scopes permit

#### Refresh Token
- Same as OAuth 2.0
- Obtain new tokens without re-authentication

### OIDC Flows

#### 1. Authorization Code Flow
Same as OAuth 2.0 but returns ID token with access token
```
1. Client redirects to authorization endpoint
2. User authenticates
3. Auth server returns authorization code
4. Client exchanges code for tokens (ID + Access + Refresh)
5. Client validates ID token
6. Client can call UserInfo endpoint with access token
```

#### 2. Implicit Flow (Deprecated)
Tokens returned directly from authorization endpoint
**Status**: Not recommended

#### 3. Hybrid Flow
Combination of Authorization Code and Implicit
**Use Case**: Complex scenarios requiring tokens at different steps

### OIDC Scopes

Standard scopes:
- `openid` (required): Indicates OIDC request
- `profile`: User's profile information
- `email`: User's email and verification status
- `address`: User's postal address
- `phone`: User's phone number

### OIDC Endpoints

- **Authorization Endpoint**: User authentication
- **Token Endpoint**: Token issuance
- **UserInfo Endpoint**: User profile information
- **JWKS Endpoint**: Public keys for token verification
- **Discovery Endpoint**: Well-known configuration

### Discovery (Well-Known Configuration)

```
GET /.well-known/openid-configuration
```

Returns JSON with all endpoints and supported features:
```json
{
  "issuer": "https://auth.example.com",
  "authorization_endpoint": "https://auth.example.com/authorize",
  "token_endpoint": "https://auth.example.com/token",
  "userinfo_endpoint": "https://auth.example.com/userinfo",
  "jwks_uri": "https://auth.example.com/.well-known/jwks.json",
  "response_types_supported": ["code", "token", "id_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"]
}
```

### When to Use OIDC

✅ User authentication for web/mobile apps
✅ Single Sign-On (SSO)
✅ Social login
✅ Modern applications
✅ APIs requiring user identity

---

## SAML 2.0

### What is SAML 2.0?

Security Assertion Markup Language (SAML) 2.0 is an XML-based framework for exchanging authentication and authorization data between parties, specifically between an Identity Provider and a Service Provider.

### Key Characteristics

- **Purpose**: Authentication and authorization
- **Format**: XML-based
- **Assertions**: SAML assertions contain auth info
- **Best For**: Enterprise SSO
- **Transport**: HTTP POST/Redirect, SOAP

### SAML Components

#### Identity Provider (IdP)
- Authenticates users
- Issues SAML assertions
- Examples: Okta, Azure AD, WSO2 IS

#### Service Provider (SP)
- Consumes SAML assertions
- Provides services to users
- Examples: Salesforce, Workday, custom apps

#### SAML Assertion
XML document containing:
- **Authentication Statement**: How and when user was authenticated
- **Attribute Statement**: User attributes (name, email, role)
- **Authorization Statement**: What user can access

### SAML Assertion Example

```xml
<saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                Version="2.0"
                ID="_abc123"
                IssueInstant="2024-02-06T12:00:00Z">
  <saml:Issuer>https://idp.example.com</saml:Issuer>
  
  <saml:Subject>
    <saml:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress">
      user@example.com
    </saml:NameID>
  </saml:Subject>
  
  <saml:Conditions NotBefore="2024-02-06T12:00:00Z"
                   NotOnOrAfter="2024-02-06T13:00:00Z">
    <saml:AudienceRestriction>
      <saml:Audience>https://sp.example.com</saml:Audience>
    </saml:AudienceRestriction>
  </saml:Conditions>
  
  <saml:AuthnStatement AuthnInstant="2024-02-06T12:00:00Z">
    <saml:AuthnContext>
      <saml:AuthnContextClassRef>
        urn:oasis:names:tc:SAML:2.0:ac:classes:Password
      </saml:AuthnContextClassRef>
    </saml:AuthnContext>
  </saml:AuthnStatement>
  
  <saml:AttributeStatement>
    <saml:Attribute Name="email">
      <saml:AttributeValue>user@example.com</saml:AttributeValue>
    </saml:Attribute>
    <saml:Attribute Name="role">
      <saml:AttributeValue>admin</saml:AttributeValue>
    </saml:Attribute>
  </saml:AttributeStatement>
</saml:Assertion>
```

### SAML Flows

#### SP-Initiated Flow
```
1. User accesses SP application
2. SP generates SAML AuthnRequest
3. SP redirects user to IdP with AuthnRequest
4. User authenticates at IdP
5. IdP generates SAML Response with Assertion
6. IdP redirects user back to SP with Response
7. SP validates assertion
8. User is logged in
```

#### IdP-Initiated Flow
```
1. User logs into IdP portal
2. User selects application
3. IdP generates SAML Response
4. IdP redirects user to SP with Response
5. SP validates assertion
6. User is logged in
```

### SAML Bindings

- **HTTP Redirect**: SAML request in URL parameters
- **HTTP POST**: SAML request in HTML form
- **SOAP**: SAML over SOAP protocol
- **Artifact**: Reference to actual SAML message

### SAML Metadata

XML document describing IdP or SP capabilities:
```xml
<EntityDescriptor entityID="https://idp.example.com">
  <IDPSSODescriptor>
    <SingleSignOnService 
      Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect"
      Location="https://idp.example.com/sso"/>
    <KeyDescriptor use="signing">
      <!-- X.509 Certificate -->
    </KeyDescriptor>
  </IDPSSODescriptor>
</EntityDescriptor>
```

### Common SAML Issues

1. **Clock Skew**: Time mismatch between IdP and SP
2. **Certificate Expiration**: Invalid signing certificates
3. **Attribute Mapping**: Mismatched attribute names
4. **Signature Validation**: Failed signature checks
5. **Relay State**: Lost return URL information

### When to Use SAML 2.0

✅ Enterprise SSO
✅ B2B integrations
✅ Legacy systems
✅ Strong security requirements
✅ Established enterprise IdPs (Active Directory)

---

## Detailed Comparison

### Security

| Feature | OAuth 2.0 | OIDC | SAML 2.0 |
|---------|-----------|------|----------|
| **Token Security** | Bearer tokens | JWT with signature | XML signatures |
| **Transport Security** | HTTPS required | HTTPS required | HTTPS required |
| **Token Binding** | Optional | Optional | Built-in |
| **Message Signing** | Token level | Token level | Message level |
| **Message Encryption** | TLS | TLS | TLS + XML encryption |

### Implementation Complexity

| Aspect | OAuth 2.0 | OIDC | SAML 2.0 |
|--------|-----------|------|----------|
| **Learning Curve** | Medium | Medium | High |
| **Libraries** | Many | Many | Fewer |
| **Debugging** | Easy (JSON) | Easy (JSON) | Hard (XML) |
| **Setup Time** | Quick | Quick | Long |

### Use Cases

#### OAuth 2.0
- ✅ API authorization
- ✅ Third-party app access
- ✅ Delegated permissions
- ✅ Microservices
- ❌ User authentication alone

#### OpenID Connect
- ✅ User login/authentication
- ✅ Single Sign-On
- ✅ Social login
- ✅ Mobile apps
- ✅ Modern web apps
- ✅ Microservices with user context

#### SAML 2.0
- ✅ Enterprise SSO
- ✅ B2B federation
- ✅ Compliance requirements
- ✅ Legacy system integration
- ❌ Mobile apps (difficult)
- ❌ APIs (not ideal)

---

## Migration Paths

### SAML to OIDC

**Reasons to Migrate**:
- Simpler implementation
- Better mobile support
- Modern API design
- JSON instead of XML
- Growing ecosystem

**Considerations**:
- Maintain SAML during transition
- Support both protocols temporarily
- Update client applications
- Migrate attribute mapping

### OAuth 2.0 to OIDC

**Reasons to Add OIDC**:
- Need user authentication
- Want standardized ID tokens
- Require user profile information

**Approach**:
- OIDC is built on OAuth 2.0
- Add OpenID Connect flows
- Issue ID tokens alongside access tokens
- Minimal changes to existing OAuth implementation

---

## Quick Decision Guide

### Choose OAuth 2.0 when:
- Building API authorization
- Don't need user authentication
- Implementing delegated access
- Machine-to-machine communication

### Choose OpenID Connect when:
- Need user authentication
- Building SSO for modern apps
- Want user profile information
- Developing mobile apps
- Need social login

### Choose SAML 2.0 when:
- Enterprise SSO requirement
- B2B integration
- Compliance mandates SAML
- Integrating with existing SAML infrastructure
- Strong XML signing/encryption requirements

---

## Conclusion

- **OAuth 2.0**: Authorization framework, not for authentication alone
- **OpenID Connect**: Modern authentication protocol, built on OAuth 2.0
- **SAML 2.0**: Enterprise authentication, XML-based, more complex

**Recommendation for New Projects**: 
Start with **OpenID Connect (OIDC)** for most modern applications. It provides both authentication and authorization with wide industry support and excellent tooling.

---

## Resources

### OAuth 2.0
- [RFC 6749 - OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749)
- [OAuth 2.0 Security Best Practices](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics)

### OpenID Connect
- [OpenID Connect Specification](https://openid.net/connect/)
- [OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html)

### SAML 2.0
- [SAML 2.0 Technical Overview](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html)
- [SAML 2.0 Specifications](https://wiki.oasis-open.org/security)

---

**Last Updated**: February 2024
