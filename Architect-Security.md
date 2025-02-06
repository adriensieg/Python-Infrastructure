# Goal

Develop a secure web application using Python/Flask for the backend and HTML/JavaScript for the frontend. 
This application will implement CRUD (Create, Read, Update, Delete) functionality, allowing users to create new entries, update, read, and delete existing ones.

Entries will be displayed in a tabular format on the frontend. Each row will include two action buttons—one for updating and another for deleting the respective entry. Additionally, a button positioned at the top center of the webpage will allow users to add new entries.

The application will use Google Cloud Firestore as its database. 
It will be deployed on Google Cloud Run using Docker, with a CI/CD pipeline managed via Cloud Build.

# User Management & Security

This system will support multiple users, each having an isolated session to ensure strict data segregation. Each user will have access only to their own personalized database to prevent unauthorized data access. Session management must be robustly secured, incorporating secure cookies for authentication and personalization.

# Security Objectives & Measures

The primary objective of this application is to maximize security. To achieve this, I plan to implement:
- **Strong authentication mechanisms**, including <mask>Identity-Aware Proxy (IAP)</mask> for secure authentication.
- **Comprehensive input sanitization** to mitigate security vulnerabilities such as <mask>XSS</mask>, <mask>SQL/NoSQL injection</mask>, and <mask>CSRF attacks</mask>.
- **Strict HTTP security headers** to enhance protection against various <mask>web threats</mask>.
- **Secure communication protocols**, including mandatory <mask>HTTPS enforcement</mask> and proper <mask>CORS configuration</mask>.
- **Robust API security**, ensuring <mask>JWT-based authentication</mask> and </mask>fine-grained role-based access control (RBAC)</mask>.
- **Rate limiting on all endpoints**
- **Comprehensive error handling** and **Security event logging**, Logging and alerting mechanisms for anomaly detection
- Cloud Run Deployment Security – IAM policies, container security, secrets management, and network security.
- CI/CD Pipeline Security (Cloud Build) – Secure deployments, static code analysis, and artifact scanning.

## Back-End
### Authentication & Authorization

- 👉 Use **Google Identity-Aware Proxy (IAP)** for authentication
- 👉 Restrict **access** using **role-based** or **attribute-based access control** (**RBAC**/**ABAC**).
- 👉 Configure **session management**

```python 
from flask import Flask
app = Flask(__name__)
app.config.update(
    SESSION_COOKIE_SECURE=True,
    SESSION_COOKIE_HTTPONLY=True,
    SESSION_COOKIE_SAMESITE='Strict',
    PERMANENT_SESSION_LIFETIME=timedelta(hours=1)
)
```

### Input Validation & Sanitization

- 👉 Use Flask’s built-in **request validation**
- 👉 Leverage **Marshmallow** (**Pydantic**) for **schema validation**
- 👉 Implement **server-side input sanitization** to prevent NoSQL injection
    - Validate and sanitize all incoming data to prevent SQL/NoSQL injections
    - Set strict schema rules for Firestore entries.
    - Avoid dynamic queries with user inputs
    - Use Firestore’s structured queries instead of raw queries
 
```python 
from pydantic import BaseModel, validator
from typing import Optional

class EntrySchema(BaseModel):
    title: str
    content: str
    user_id: str
    
    @validator('title')
    def title_must_be_valid(cls, v):
        if len(v) > 200:
            raise ValueError('Title too long')
        return v.strip()
```
- 👉 **Restrict allowed content** types for user inputs
- 👉 Use **parameterized queries** for Firestore - to prevent SQL injection

```python
def get_user_entries(user_id: str):
    return db.collection('entries').where('user_id', '==', user_id).get()
```

- 👉Enforce **rate limiting** using **Flask-Limiter** to prevent **DoS attacks**.

```python
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

limiter = Limiter(
    app,
    key_func=get_remote_address,
    default_limits=["200 per day", "50 per hour"]
)
```

- 👉 Disable Unnecessary HTTP Methods
    - Allow only ```GET```, ```POST```, ```PUT```, and ```DELETE``` where necessary.

- 👉 MIME Type Validation: Verify file types before saving them.
- 👉 Restrict Executable Files: Block .exe, .sh, .php, etc.
- 👉 Use a CDN or Separate Storage: Store user-uploaded files in S3/GCS instead of the main server.

### API Security

- 👉 Implement **secure API endpoints** using **HTTPS** with **HSTS**.
    - Redirect all HTTP traffic to HTTPS using Cloud Load Balancer

- 👉 Use **JWT-based authentication** with **short-lived tokens**.
- 👉 Restrict **CORS** to **trusted domains**

```python
from flask_cors import CORS
CORS(app, resources={r"/*": {"origins": ["https://yourfrontend.com"]}})
```
- 👉 Register **blueprints**

```python
from app.routes import main_bp, auth_bp, api_bp

# Initialize blueprints
main_bp = Blueprint('main', __name__)
auth_bp = Blueprint('auth', __name__)
api_bp = Blueprint('api', __name__)

@auth_bp.route('/logout')
def logout():
  ...
  session.clear()
  return jsonify({'message': 'Logged out successfully'})

@api_bp.route('/entries', methods=['GET'])
@require_auth
@limiter.limit("100 per minute")
def get_entries():
  ...
  return jsonify({'error': 'Failed to fetch entries'}), 500
```

### Secure Cookies & Sessions
- 👉 Set **Secure**, **HttpOnly**, and **SameSite=strict** on cookies.
- 👉 Store **session data** in **Firestore** or Redis instead of client-side cookies.
- 👉 Use **Flask-Talisman** to enforce strict **HTTP security headers**.

- Implement Security Headers

```python
response.headers['X-Content-Type-Options'] = 'nosniff'
response.headers['X-Frame-Options'] = 'SAMEORIGIN'
response.headers['X-XSS-Protection'] = '1; mode=block'
response.headers['Strict-Transport-Security'] = 'max-age=31536000; includeSubDomains'
response.headers['Cache-Control'] = 'no-store, no-cache, must-revalidate, max-age=0'
```

- 👉 Add **security headers**

```python
from flask_talisman import Talisman

Talisman(app,
    force_https=True,
    strict_transport_security=True,
    session_cookie_secure=True,
    content_security_policy={
        'default-src': "'self'",
        'script-src': "'self' 'unsafe-inline' 'unsafe-eval'",
        'style-src': "'self' 'unsafe-inline'",
    }
)
```

```python
from flask import Flask
from flask_talisman import Talisman

def configure_security(app: Flask):
    csp = {
        'default-src': [
            '\'self\''
        ],
        'script-src': [
            '\'self\'',
            '\'unsafe-inline\''  # Allow inline scripts for development
        ],
        'style-src': [
            '\'self\''
        ]
    }
    Talisman(app, content_security_policy=csp)

    @app.after_request
    def set_security_headers(response):
        response.headers['X-Content-Type-Options'] = 'nosniff'
        response.headers['X-Frame-Options'] = 'SAMEORIGIN'
        response.headers['X-XSS-Protection'] = '1; mode=block'
        response.headers['Strict-Transport-Security'] = 'max-age=31536000; includeSubDomains'
        response.headers['Cache-Control'] = 'no-store, no-cache, must-revalidate, max-age=0'
        return response
```

- 👉 Session Expiry & Revocation: Implement session expiration and logout functionalities properly.

### Prevent CSRF Attacks
- 👉 Use **Flask-WTF** with **CSRF protection** enabled.
- 👉 Include **CSRF tokens** in all POST/PUT/DELETE requests.

### Logging & Monitoring
- 👉 Centralized Logging: Use Flask-Logging with structured logs.
- 👉 Intrusion Detection: Monitor logs for unusual activity.
- 👉 Real-time Alerts: Integrate monitoring tools like Prometheus, Grafana, or ELK Stack.

### Code Security & Hardening
- 👉 Automatic Dependency Updates: Use pip-audit or dependabot to check for vulnerable dependencies.
- 👉 Limit Flask Extensions: Use only well-maintained Flask extensions.
  
## Front-End

- 👉 Avoid Inline JavaScript: Move scripts to external files.
- 👉 Use Subresource Integrity (SRI): Ensure external resources (CDNs) are not tampered with.
- 👉 Disable Client-Side Debugging: Remove debug messages from production.

### Session Management

- 👉 Avoid Storing Sensitive Data in Local Storage
    - Use ```HttpOnly``` and ```Secure cookies``` for session management.
    - Ensure session cookies have Secure, HttpOnly, and SameSite=Strict attributes.
    - Store sensitive data in HttpOnly cookies (not in local storage).

- 👉 Implement session expiration and automatic logout after inactivity
  
 ### XSS Prevention 
- 👉 Sanitize all user inputs before rendering
- 👉 Sanitize user-generated content using DOMPurify
- 👉 Avoid ```innerHTML``` and use ```textContent``` or ```DOMPurify``` for rendering.

```javascript
function sanitizeInput(input) {
    const div = document.createElement('div');
    div.textContent = input;
    return div.innerHTML;
}
```

```javascript
var cleanHtml = DOMPurify.sanitize(dirtyHtml);
```

- 👉 Use **Content Security Policy (CSP)** headers to restrict **inline scripts**

```javascript
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' https://apis.google.com;">
```

```html
<meta http-equiv="Content-Security-Policy" 
    content="default-src 'self'; 
    script-src 'self' 'unsafe-inline' 'unsafe-eval'; 
    style-src 'self' 'unsafe-inline';">
```
### CSRF Protection

- 👉 Implement CSRF tokens

```javascript
async function makeRequest(url, method, data) {
    const response = await fetch(url, {
        method: method,
        headers: {
            'Content-Type': 'application/json',
            'X-CSRF-Token': document.querySelector('meta[name="csrf-token"]').content
        },
        credentials: 'same-origin',
        body: JSON.stringify(data)
    });
    return response.json();
}
```

- 👉 Prevent Clickjacking Attacks
    - Set ```X-Frame-Options``` in the response headers:

```python
@app.after_request
def set_security_headers(response):
    response.headers["X-Frame-Options"] = "DENY"
    return response
```

## Database Security

### Security Rules

- 👉 Restrict Access Based on User Identity
- 👉 Validate Data on Firestore Side

```powershell
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /entries/{entryId} {
      allow read: if request.auth != null 
                 && request.auth.uid == resource.data.user_id;
      allow write: if request.auth != null 
                  && request.auth.uid == request.resource.data.user_id;
      
      function validateEntry() {
        let incoming = request.resource.data;
        return incoming.size() <= 1000000
               && incoming.title.size() <= 200
               && incoming.content.size() <= 50000;
      }
    }
  }
}
```

- 👉 ORM-Level Security: Define permissions in ORM models to restrict unauthorized access to data.

### Encryption

- 👉 Use Firestore’s built-in encryption at rest

- 👉 Use IAM to Restrict Access to Firestore

```gcloud
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:you@example.com \
  --role=roles/datastore.user
```

- 👉 Implement field-level encryption for sensitive data

```python
from cryptography.fernet import Fernet

def encrypt_field(data: str) -> str:
    key = Fernet.generate_key()
    f = Fernet(key)
    return f.encrypt(data.encode()).decode()
```

## Deployment Security

- 👉 HTTPS with HSTS: Enforce HTTPS and use Strict-Transport-Security.
- 👉Reverse Proxy (NGINX): Serve Flask via Gunicorn + Nginx with security headers.
- 👉 Container Security: If using Docker, scan images for vulnerabilities.
- 👉 Secrets Management: Use environment variables, AWS Secrets Manager, or HashiCorp Vault.
















