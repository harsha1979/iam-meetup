# IAM Samples & Code Examples

This directory contains practical code samples and implementation examples for various IAM scenarios.

## 📁 Directory Structure

```
samples/
├── oauth2/                 # OAuth 2.0 implementations
├── oidc/                   # OpenID Connect examples
├── saml/                   # SAML integration samples
├── jwt/                    # JWT authentication examples
├── mfa/                    # Multi-factor authentication
├── passwordless/           # Passwordless authentication
└── integrations/           # Third-party integrations
```

## 🔐 Available Samples

### OAuth 2.0 Examples

#### Authorization Code Flow
```javascript
// Example: OAuth 2.0 Authorization Code Flow with PKCE
const express = require('express');
const crypto = require('crypto');
const axios = require('axios');

const app = express();

// Configuration
const config = {
  clientId: 'YOUR_CLIENT_ID',
  authorizationEndpoint: 'https://your-idp.com/oauth2/authorize',
  tokenEndpoint: 'https://your-idp.com/oauth2/token',
  redirectUri: 'http://localhost:3000/callback',
  scope: 'openid profile email'
};

// Generate PKCE challenge
function generatePKCE() {
  const verifier = crypto.randomBytes(32).toString('base64url');
  const challenge = crypto
    .createHash('sha256')
    .update(verifier)
    .digest('base64url');
  
  return { verifier, challenge };
}

// Step 1: Redirect to authorization endpoint
app.get('/login', (req, res) => {
  const state = crypto.randomBytes(16).toString('hex');
  const { verifier, challenge } = generatePKCE();
  
  // Store verifier and state in session (simplified)
  req.session = { verifier, state };
  
  const authUrl = new URL(config.authorizationEndpoint);
  authUrl.searchParams.append('response_type', 'code');
  authUrl.searchParams.append('client_id', config.clientId);
  authUrl.searchParams.append('redirect_uri', config.redirectUri);
  authUrl.searchParams.append('scope', config.scope);
  authUrl.searchParams.append('state', state);
  authUrl.searchParams.append('code_challenge', challenge);
  authUrl.searchParams.append('code_challenge_method', 'S256');
  
  res.redirect(authUrl.toString());
});

// Step 2: Handle callback and exchange code for token
app.get('/callback', async (req, res) => {
  const { code, state } = req.query;
  
  // Verify state
  if (state !== req.session.state) {
    return res.status(400).send('Invalid state parameter');
  }
  
  try {
    // Exchange authorization code for tokens
    const response = await axios.post(config.tokenEndpoint, {
      grant_type: 'authorization_code',
      code: code,
      redirect_uri: config.redirectUri,
      client_id: config.clientId,
      code_verifier: req.session.verifier
    });
    
    const { access_token, id_token, refresh_token } = response.data;
    
    // Store tokens securely
    req.session.tokens = { access_token, id_token, refresh_token };
    
    res.send('Authentication successful!');
  } catch (error) {
    res.status(500).send('Token exchange failed');
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

### OpenID Connect Example

#### OIDC with ID Token Validation
```javascript
const jwt = require('jsonwebtoken');
const jwksClient = require('jwks-rsa');

// JWKS client for key verification
const client = jwksClient({
  jwksUri: 'https://your-idp.com/.well-known/jwks.json'
});

function getKey(header, callback) {
  client.getSigningKey(header.kid, (err, key) => {
    const signingKey = key.publicKey || key.rsaPublicKey;
    callback(null, signingKey);
  });
}

// Verify ID Token
function verifyIdToken(idToken) {
  return new Promise((resolve, reject) => {
    jwt.verify(idToken, getKey, {
      audience: 'YOUR_CLIENT_ID',
      issuer: 'https://your-idp.com',
      algorithms: ['RS256']
    }, (err, decoded) => {
      if (err) reject(err);
      else resolve(decoded);
    });
  });
}

// Usage
async function authenticateUser(idToken) {
  try {
    const claims = await verifyIdToken(idToken);
    console.log('User authenticated:', claims);
    return {
      userId: claims.sub,
      email: claims.email,
      name: claims.name
    };
  } catch (error) {
    console.error('Token verification failed:', error);
    throw error;
  }
}
```

### JWT Authentication

#### Simple JWT Authentication Middleware
```javascript
const jwt = require('jsonwebtoken');

const SECRET_KEY = process.env.JWT_SECRET || 'your-secret-key';

// Generate JWT
function generateToken(user) {
  const payload = {
    sub: user.id,
    email: user.email,
    role: user.role
  };
  
  return jwt.sign(payload, SECRET_KEY, {
    expiresIn: '1h',
    issuer: 'your-app',
    audience: 'your-app-users'
  });
}

// Verify JWT Middleware
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }
  
  jwt.verify(token, SECRET_KEY, (err, user) => {
    if (err) {
      return res.status(403).json({ error: 'Invalid or expired token' });
    }
    req.user = user;
    next();
  });
}

// Protected route example
app.get('/api/protected', authenticateToken, (req, res) => {
  res.json({
    message: 'Protected resource',
    user: req.user
  });
});
```

### Multi-Factor Authentication (MFA)

#### TOTP Implementation
```javascript
const speakeasy = require('speakeasy');
const QRCode = require('qrcode');

// Generate MFA secret
function generateMFASecret(userEmail) {
  const secret = speakeasy.generateSecret({
    name: `YourApp (${userEmail})`,
    length: 32
  });
  
  return {
    secret: secret.base32,
    otpauth_url: secret.otpauth_url
  };
}

// Generate QR code
async function generateQRCode(otpauth_url) {
  try {
    const qrCode = await QRCode.toDataURL(otpauth_url);
    return qrCode;
  } catch (error) {
    throw new Error('QR code generation failed');
  }
}

// Verify TOTP token
function verifyMFAToken(secret, token) {
  return speakeasy.totp.verify({
    secret: secret,
    encoding: 'base32',
    token: token,
    window: 2 // Allow 2 time steps before/after
  });
}

// MFA enrollment endpoint
app.post('/api/mfa/enroll', authenticateToken, async (req, res) => {
  const { secret, otpauth_url } = generateMFASecret(req.user.email);
  const qrCode = await generateQRCode(otpauth_url);
  
  // Store secret in database (encrypted)
  await storeUserMFASecret(req.user.id, secret);
  
  res.json({
    secret: secret,
    qrCode: qrCode
  });
});

// MFA verification endpoint
app.post('/api/mfa/verify', authenticateToken, async (req, res) => {
  const { token } = req.body;
  const secret = await getUserMFASecret(req.user.id);
  
  if (verifyMFAToken(secret, token)) {
    res.json({ success: true, message: 'MFA verified' });
  } else {
    res.status(401).json({ success: false, message: 'Invalid MFA token' });
  }
});
```

### Passwordless Authentication

#### Magic Link Authentication
```javascript
const crypto = require('crypto');
const nodemailer = require('nodemailer');

// Generate magic link token
function generateMagicLinkToken() {
  return crypto.randomBytes(32).toString('hex');
}

// Send magic link email
async function sendMagicLink(email) {
  const token = generateMagicLinkToken();
  const expiresAt = Date.now() + 15 * 60 * 1000; // 15 minutes
  
  // Store token in database with expiration
  await storeMagicLinkToken(email, token, expiresAt);
  
  const magicLink = `https://yourapp.com/auth/verify?token=${token}`;
  
  // Send email (simplified)
  const transporter = nodemailer.createTransport({
    host: process.env.SMTP_HOST,
    port: process.env.SMTP_PORT,
    auth: {
      user: process.env.SMTP_USER,
      pass: process.env.SMTP_PASS
    }
  });
  
  await transporter.sendMail({
    from: 'noreply@yourapp.com',
    to: email,
    subject: 'Your Magic Link',
    html: `<p>Click <a href="${magicLink}">here</a> to sign in. This link expires in 15 minutes.</p>`
  });
}

// Verify magic link
app.get('/auth/verify', async (req, res) => {
  const { token } = req.query;
  
  try {
    const tokenData = await getMagicLinkToken(token);
    
    if (!tokenData) {
      return res.status(400).send('Invalid token');
    }
    
    if (Date.now() > tokenData.expiresAt) {
      return res.status(400).send('Token expired');
    }
    
    // Generate session token
    const sessionToken = generateToken({ email: tokenData.email });
    
    // Invalidate magic link token
    await deleteMagicLinkToken(token);
    
    res.cookie('session', sessionToken, {
      httpOnly: true,
      secure: true,
      sameSite: 'strict'
    });
    
    res.redirect('/dashboard');
  } catch (error) {
    res.status(500).send('Authentication failed');
  }
});
```

### API Security with OAuth 2.0

#### Resource Server Protection
```javascript
const axios = require('axios');

// Token introspection
async function introspectToken(accessToken) {
  try {
    const response = await axios.post(
      'https://your-idp.com/oauth2/introspect',
      `token=${accessToken}`,
      {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
          'Authorization': `Basic ${Buffer.from('CLIENT_ID:CLIENT_SECRET').toString('base64')}`
        }
      }
    );
    
    return response.data;
  } catch (error) {
    throw new Error('Token introspection failed');
  }
}

// OAuth 2.0 middleware for API protection
async function validateAccessToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }
  
  try {
    const introspection = await introspectToken(token);
    
    if (!introspection.active) {
      return res.status(401).json({ error: 'Token is not active' });
    }
    
    // Check scopes
    const requiredScope = 'api:read';
    if (!introspection.scope.includes(requiredScope)) {
      return res.status(403).json({ error: 'Insufficient scope' });
    }
    
    req.oauth = {
      clientId: introspection.client_id,
      userId: introspection.sub,
      scopes: introspection.scope.split(' ')
    };
    
    next();
  } catch (error) {
    res.status(500).json({ error: 'Token validation failed' });
  }
}

// Protected API endpoint
app.get('/api/data', validateAccessToken, (req, res) => {
  res.json({
    message: 'Protected data',
    user: req.oauth.userId
  });
});
```

## 🚀 Getting Started

### Prerequisites
```bash
# Install dependencies
npm install express axios jsonwebtoken jwks-rsa speakeasy qrcode nodemailer
```

### Configuration
Create a `.env` file:
```env
JWT_SECRET=your-jwt-secret-key
CLIENT_ID=your-oauth-client-id
CLIENT_SECRET=your-oauth-client-secret
IDP_URL=https://your-identity-provider.com
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=your-smtp-username
SMTP_PASS=your-smtp-password
```

## 📖 Additional Resources

- See `/resources` for more learning materials
- See `/documents` for presentations and diagrams
- Check individual sample directories for specific implementation guides

## ⚠️ Security Notes

- **Never commit secrets** to version control
- **Always use HTTPS** in production
- **Validate and sanitize** all inputs
- **Implement rate limiting** to prevent abuse
- **Use secure session management**
- **Keep dependencies updated**

---

**Happy Coding!** 🎉
