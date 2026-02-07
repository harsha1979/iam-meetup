# Getting Started with Open Source IAM

A practical guide to choosing and implementing open-source identity and access management solutions.

---

## 📋 Table of Contents

1. [Why Open Source IAM?](#why-open-source-iam)
2. [Choosing the Right IAM Solution](#choosing-the-right-iam-solution)
3. [Quick Start Guides](#quick-start-guides)
4. [Common Use Cases](#common-use-cases)
5. [Best Practices](#best-practices)
6. [Troubleshooting](#troubleshooting)

---

## Why Open Source IAM?

### Benefits

✅ **Cost-Effective**: No licensing fees, only infrastructure and support costs
✅ **Transparency**: Full access to source code for security audits
✅ **Flexibility**: Customize to meet specific requirements
✅ **Community Support**: Active communities and extensive documentation
✅ **No Vendor Lock-in**: Freedom to migrate or modify
✅ **Innovation**: Rapid feature development through community contributions

### Considerations

⚠️ **Support**: Community support vs. paid enterprise support
⚠️ **Maintenance**: Responsibility for updates and security patches
⚠️ **Expertise**: May require in-house IAM knowledge
⚠️ **Integration**: Ensure compatibility with existing systems

---

## Choosing the Right IAM Solution

### Decision Matrix

| Criteria | Keycloak | WSO2 IS | Ory | Authelia | Authentik |
|----------|----------|---------|-----|----------|-----------|
| **Ease of Setup** | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Enterprise Features** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Cloud-Native** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **API-First Design** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ |
| **Documentation** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Community Size** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| **Customization** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |

### Use Case Recommendations

#### Enterprise SSO
**Best Choice**: Keycloak or WSO2 Identity Server
- Mature enterprise features
- SAML 2.0 support
- User federation (LDAP/AD)
- Strong community and documentation

#### Microservices Authentication
**Best Choice**: Ory or WSO2 Identity Server
- Cloud-native architecture
- API-first design
- Scalable and lightweight
- Modern standards (OIDC, OAuth 2.0)

#### Self-Hosted Personal Use
**Best Choice**: Authelia or Authentik
- Simple setup
- Low resource requirements
- Perfect for home labs
- Reverse proxy integration

#### Customer IAM (CIAM)
**Best Choice**: Keycloak, WSO2 IS, or Ory
- Social login support
- Self-service features
- Scalability
- Customizable UI/UX

#### Startups/SMBs
**Best Choice**: Keycloak or Authentik
- Quick deployment
- Good documentation
- Active community
- Feature-rich without complexity

---

## Quick Start Guides

### Keycloak Quick Start

#### Docker Deployment
```bash
# Start Keycloak in development mode
docker run -d \
  --name keycloak \
  -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:latest \
  start-dev

# Access admin console
# URL: http://localhost:8080
# Username: admin
# Password: admin
```

#### Basic Configuration
1. **Create a Realm**
   - Login to admin console
   - Click "Add Realm"
   - Name: "my-app-realm"

2. **Create a Client**
   - Select your realm
   - Clients → Create
   - Client ID: "my-app"
   - Client Protocol: "openid-connect"
   - Root URL: "http://localhost:3000"

3. **Create a User**
   - Users → Add User
   - Username: "testuser"
   - Email: "test@example.com"
   - Set password in Credentials tab

4. **Test Authentication**
   ```bash
   # Get access token
   curl -X POST http://localhost:8080/realms/my-app-realm/protocol/openid-connect/token \
     -d "client_id=my-app" \
     -d "username=testuser" \
     -d "password=yourpassword" \
     -d "grant_type=password"
   ```

---

### WSO2 Identity Server Quick Start

#### Docker Deployment
```bash
# Pull and run WSO2 IS
docker run -d \
  --name wso2is \
  -p 9443:9443 \
  wso2/wso2is:latest

# Access admin console
# URL: https://localhost:9443/carbon
# Username: admin
# Password: admin
```

#### Basic Configuration
1. **Create Service Provider**
   - Login to management console
   - Service Providers → Add
   - Name: "My App"
   - Configure OAuth/OIDC

2. **Configure OAuth Settings**
   - Inbound Authentication → OAuth/OpenID Connect
   - Callback URL: "http://localhost:3000/callback"
   - Enable desired grant types

3. **Create User**
   - Users and Roles → Users → Add User
   - Username: "testuser"
   - Set password and assign roles

---

### Ory Quick Start

#### Ory Kratos (Identity Management)
```bash
# Clone Ory Kratos quickstart
git clone https://github.com/ory/kratos.git
cd kratos

# Start with Docker Compose
docker-compose -f quickstart.yml up

# Access:
# Kratos: http://localhost:4433
# Self-service UI: http://localhost:4455
```

#### Ory Hydra (OAuth 2.0 Server)
```bash
# Clone Ory Hydra quickstart
git clone https://github.com/ory/hydra.git
cd hydra

# Start with Docker Compose
docker-compose up

# Create OAuth client
docker-compose exec hydra \
  hydra clients create \
    --endpoint http://localhost:4445 \
    --id my-app \
    --secret secret \
    --grant-types authorization_code,refresh_token \
    --response-types code \
    --scope openid,offline \
    --callbacks http://localhost:3000/callback
```

---

### Authelia Quick Start

#### Docker Compose Setup
```yaml
version: '3.8'

services:
  authelia:
    image: authelia/authelia:latest
    container_name: authelia
    volumes:
      - ./config:/config
    ports:
      - 9091:9091
    environment:
      - TZ=UTC
    restart: unless-stopped
```

#### Minimal Configuration (`config/configuration.yml`)
```yaml
server:
  host: 0.0.0.0
  port: 9091

log:
  level: info

authentication_backend:
  file:
    path: /config/users_database.yml

access_control:
  default_policy: deny
  rules:
    - domain: "*.example.com"
      policy: two_factor

session:
  secret: changeme-secret-key
  domain: example.com

storage:
  local:
    path: /config/db.sqlite3

notifier:
  filesystem:
    filename: /config/notification.txt
```

---

## Common Use Cases

### Use Case 1: Single Sign-On (SSO)

**Scenario**: Multiple applications, one login

**Solution with Keycloak**:
1. Deploy Keycloak
2. Create realm for organization
3. Register each application as a client
4. Configure SAML or OIDC for each client
5. Users login once, access all apps

**Benefits**:
- Improved user experience
- Centralized user management
- Reduced password fatigue
- Better security (one strong password)

---

### Use Case 2: API Security

**Scenario**: Protect REST APIs with OAuth 2.0

**Solution with Ory Hydra**:
1. Deploy Ory Hydra as OAuth 2.0 server
2. Register API gateway as client
3. Issue access tokens to consumers
4. Validate tokens at API gateway
5. Implement scope-based permissions

**Implementation**:
```javascript
// Express.js middleware
async function validateToken(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  
  // Introspect token with Hydra
  const response = await fetch('http://hydra:4445/oauth2/introspect', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: `token=${token}`
  });
  
  const data = await response.json();
  
  if (data.active) {
    req.oauth = data;
    next();
  } else {
    res.status(401).json({ error: 'Invalid token' });
  }
}
```

---

### Use Case 3: Customer Identity (CIAM)

**Scenario**: User registration, login, profile management for customer-facing app

**Solution with WSO2 Identity Server**:
1. Deploy WSO2 IS
2. Enable self-registration
3. Configure social login providers
4. Customize login/registration UI
5. Enable email verification
6. Implement password reset flow

**Features**:
- Social login (Google, Facebook, GitHub)
- Email verification
- Password reset
- Profile management
- Consent management (GDPR)

---

### Use Case 4: Multi-Factor Authentication

**Scenario**: Add MFA to existing application

**Solution with Keycloak**:
1. Enable OTP in Keycloak authentication flow
2. Configure TOTP as required step
3. Users enroll during first login
4. Verify OTP on each login

**Configuration**:
- Authentication → Flows → Browser Flow
- Add "OTP Form" execution
- Set to "Required"
- Users see QR code for enrollment

---

## Best Practices

### Security

1. **Use HTTPS Everywhere**
   ```nginx
   # Nginx configuration
   server {
       listen 443 ssl http2;
       ssl_certificate /path/to/cert.pem;
       ssl_certificate_key /path/to/key.pem;
       ssl_protocols TLSv1.2 TLSv1.3;
   }
   ```

2. **Secure Token Storage**
   - Use HttpOnly cookies for web apps
   - Use secure storage for mobile apps
   - Never store tokens in localStorage

3. **Implement Token Rotation**
   - Use short-lived access tokens (15-60 minutes)
   - Implement refresh token rotation
   - Revoke tokens on logout

4. **Rate Limiting**
   ```javascript
   // Express rate limit
   const rateLimit = require('express-rate-limit');
   
   const loginLimiter = rateLimit({
     windowMs: 15 * 60 * 1000, // 15 minutes
     max: 5, // 5 attempts
     message: 'Too many login attempts'
   });
   
   app.post('/login', loginLimiter, handleLogin);
   ```

### High Availability

1. **Database Replication**
   - Use clustered database
   - Regular backups
   - Test restore procedures

2. **Load Balancing**
   ```yaml
   # HAProxy configuration
   backend keycloak
       balance roundrobin
       server kc1 keycloak1:8080 check
       server kc2 keycloak2:8080 check
       server kc3 keycloak3:8080 check
   ```

3. **Session Replication**
   - Configure sticky sessions or
   - Use external session storage (Redis)

### Monitoring

1. **Metrics to Track**
   - Login success/failure rates
   - Token issuance rate
   - Response times
   - Error rates
   - Active sessions

2. **Logging**
   ```yaml
   # Structured logging example
   {
     "timestamp": "2024-02-06T12:00:00Z",
     "level": "INFO",
     "event": "user_login",
     "user_id": "user123",
     "ip": "192.168.1.100",
     "success": true
   }
   ```

3. **Alerts**
   - Failed login spikes
   - Token validation failures
   - Certificate expiration
   - High response times

---

## Troubleshooting

### Common Issues

#### Issue: Token Validation Failing

**Symptoms**: 401 errors, "Invalid token" messages

**Possible Causes**:
- Expired token
- Wrong signing key
- Clock skew between services
- Token used before valid time (nbf claim)

**Solutions**:
```bash
# Check token contents
echo "TOKEN" | cut -d'.' -f2 | base64 -d | jq

# Verify clock synchronization
ntpdate -q pool.ntp.org

# Check certificate validity
openssl x509 -in cert.pem -text -noout
```

#### Issue: SAML Authentication Failing

**Symptoms**: SAML errors, blank page after login

**Possible Causes**:
- Certificate mismatch
- Clock skew
- Incorrect metadata
- Attribute mapping issues

**Solutions**:
1. Enable SAML tracer in browser
2. Check certificate validity and match
3. Verify metadata exchange
4. Check attribute mappings
5. Review SAML assertion in tracer

#### Issue: Slow Login Performance

**Symptoms**: Long wait times during authentication

**Possible Causes**:
- Database queries slow
- LDAP/AD connection slow
- Too many custom extensions
- Insufficient resources

**Solutions**:
1. Enable query logging
2. Check database indexes
3. Monitor resource usage
4. Profile custom code
5. Optimize LDAP queries

---

## Migration Guides

### From Basic Auth to OAuth 2.0

**Step 1**: Deploy IAM solution
**Step 2**: Register existing app as client
**Step 3**: Update app to use OAuth flow
**Step 4**: Migrate users (if needed)
**Step 5**: Deprecate basic auth

### From Proprietary to Open Source

**Step 1**: Assess current features used
**Step 2**: Map to open-source equivalent
**Step 3**: Set up parallel system
**Step 4**: Gradually migrate applications
**Step 5**: Migrate users (with SSO bridge if possible)
**Step 6**: Deprecate old system

---

## Resources

### Official Documentation
- [Keycloak Docs](https://www.keycloak.org/documentation)
- [WSO2 IS Docs](https://is.docs.wso2.com/)
- [Ory Docs](https://www.ory.sh/docs/)
- [Authelia Docs](https://www.authelia.com/docs/)

### Community Support
- [Keycloak Discourse](https://keycloak.discourse.group/)
- [WSO2 Slack](https://wso2-community.slack.com/)
- [Ory Community](https://www.ory.sh/chat)
- [Reddit r/selfhosted](https://www.reddit.com/r/selfhosted/)

### Learning Resources
- OAuth 2.0 Playground
- JWT.io Debugger
- SAML Tracer Browser Extension

---

## Next Steps

1. **Evaluate Requirements**: Understand your use cases
2. **Choose Solution**: Based on requirements and constraints
3. **POC**: Deploy in development environment
4. **Test**: Validate with sample applications
5. **Scale**: Plan for production deployment
6. **Monitor**: Implement observability
7. **Iterate**: Continuously improve based on feedback

---

**Good luck with your IAM implementation!** 🚀

For questions and discussions, join the community or open an issue in this repository.
