# Daily Flask Application

- 👉 [Web Security Headers](https://github.com/adriensieg/Python-Infrastructure/blob/master/Architect-Daily-Flask-App.md#web-security-headers) - to protect data during transmission by ensuring secure connections and preventing malicious attacks
- 👉 [CORS]((https://github.com/adriensieg/Python-Infrastructure/blob/master/Architect-Daily-Flask-App.md#web-security-headers))- controls how websites share data across different origins, preventing unauthorized access during data transmission
- 👉 Blueprints for Modularity – Clean separation of concerns
- 👉 Rate Limiting✔️ Rate Limiting (API abuse protection)
- 👉 Session Security
- 👉 Logging
- 👉 Async - for I/O
- 👉 Pydantic
- 👉 Try / except
- 👉 except requests.exceptions.RequestException as e:
- Protect Cookie Session

- 👉 [Pydantic Model](https://github.com/adriensieg/Python-Infrastructure/blob/master/Architect-Daily-Flask-App.md#define-pydantic-models) - to define models you can use (and reuse) to verify that data conforms to the format you expect before you store or process it.
  
- 👉 Role-Based Access Control (RBAC) – Restrict actions based on user roles.✔️ Role-based access control (RBAC using JWT roles)
- 👉 Multi-Factor Authentication (MFA) – Enforce additional authentication layers.

✔️ JWT-based authentication (Signed tokens with expiry)
  ✔️ CSRF protection (via Flask-WTF CSRF)
  ✔️ Strict Content Security Policy (CSP) (Mitigates XSS)
  ✔️ Strict HTTP headers (HSTS, X-Frame-Options, Referrer-Policy)


✔️ Secure file uploads (Whitelist + UUID file names)
✔️ Structured logging (Request tracking)

✔️ Ensure CSRF is enforced on all forms and API calls.
  - Use Flask-WTF CSRF to enforce token-based protection.
  - Apply SameSite cookies to prevent CSRF attacks.

✔️ Prevent malicious script injection & data exfiltration
  - Enforce a strict CSP policy with Flask-Talisman
  - Block inline scripts & only allow trusted domains.

✔️ Enforced CSRF protection
✔️ Implemented strong CSP & Security Headers
✔️ JWT-based authentication with Role-Based Access
✔️ Secure file uploads with type validation
✔️ Rate limiting to prevent abuse
✔️ Secure logging & monitoring

✅ Blueprints for Modularity – Clean separation of concerns
✅ JWT Authentication – Each request is authenticated using a signed token
✅ Strict Data Validation – Prevents bad data using Pydantic
✅ Security Best Practices –
🔹 Rate Limiting (Mitigates abuse)
🔹 CORS & CSRF Protection (Secures against cross-site attacks)
🔹 Secure HTTP Headers (Strict CSP, HSTS, XSS protection)
✅ Environment Variables – No hardcoded secrets
✅ Optimized routes.py – Clean, maintainable, and high-performance


## Project Layout
```
secure_flask_app/
│── config/
│   ├── config.py          # Application settings
│── app.py                 # Main entry point
│── extensions.py          # Security & Flask extensions
│── blueprints/
│   ├── auth.py            # Authentication with JWT
│   ├── items.py           # Secure CRUD operations
│── utils/
│   ├── security.py        # Security helper functions
│── templates/
│── logs/
│── requirements.txt
│── .env
```

## CORS - Cross-Origin Resource Sharing

- **Goal**: Prevents unauthorized web applications from interacting with the backend while allowing trusted sources to communicate securely.
- **CORS_ORIGIN**S: Defines which domains are allowed to make cross-origin requests.

```python
app = Flask(__name__)
app = Flask(__name__, static_folder="static", template_folder="templates")

CORS(app, resources={r"/api/*": {"origins": "https://trusted-domain.com"}})
```

## CSRF Protection (Secures against cross-site attacks)

**CSRF (Cross-Site Request Forgery)** is an attack where a malicious website tricks a user’s browser into making unintended requests to another site where the user is authenticated.

- Ensures that **all form submissions** and **API requests** (especially those modifying data) include a **CSRF token** to **verify that the request comes from a trusted source**.
- Prevents attackers from **performing unauthorized actions** on behalf of authenticated users.
- **Blocks malicious requests** that lack a valid CSRF token.
- Enhances web application security by requiring **server-side validation of the CSRF token**.

```python
app = Flask(__name__)
app = Flask(__name__, static_folder="static", template_folder="templates")

csrf = CSRFProtect(app)
```

- ✅ Recommended for forms and state-changing requests (POST, PUT, DELETE).
- ❌ Not necessary for GET requests since they should not change state.

```javascript
API Request with CSRF Token (for AJAX requests)

fetch('/api/submit', {
    method: 'POST',
    headers: {
        'X-CSRFToken': getCSRFToken()  // Function to retrieve CSRF token
    },
    body: JSON.stringify({ data: 'example' })
});
```

## Cookie Session

- **Goal**: Enhances web application security by protecting session cookies from theft and unauthorized access.
  - **SESSION_COOKIE_SECURE**: Ensures cookies are only sent over HTTPS.
  - **SESSION_COOKIE_HTTPONLY**: Prevents JavaScript from accessing session cookies, mitigating XSS attacks.
  - **SESSION_COOKIE_SAMESITE**: Restricts cookies to same-site requests, reducing CSRF risks.

```python
SESSION_COOKIE_SECURE = True
SESSION_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Strict'
```

## Secret Key for Flask Session

- **Goal**: Ensures that sensitive security operations within the application are protected from tampering

The ```SECRET_KEY``` is used for cryptographic operations, including:
- Securing **session cookies**
- Protecting against **Cross-Site Request Forgery (CSRF) attacks**.
- **Signing** and **verifying tokens** (e.g., Flask sessions, JWT, or other secure data storage mechanisms).

## Rate Limiting

```python
# Rate Limiting

## Should be filled in config/config.py
RATE_LIMITS = {
    "global": "100 per minute",
    "api": "10 per minute"
}

## Should be filled in extensions.py
from flask_limiter import Limiter
limiter = Limiter(key_func=lambda: request.remote_addr)

## Should be filled in app.py
from extensions import init_extensions

@items_bp.route('/api/items', methods=['POST'])
@limiter.limit("5 per minute")
@token_required
def create_item():
  """ Function to create a item in Firestore"""
    return
```

## Content Security Policy (CSP)

- **Goal**: CSP is a browser security feature that prevents malicious scripts from running in your application, reducing the risk of:
  - Cross-Site Scripting (XSS) attacks
  - Data injection attacks
  - Code execution from untrusted sources

🔹 Goal of This Configuration
- ```default-src: 'self'```
  - Restricts all content (scripts, styles, images, etc.) to be loaded only from the same origin (your own domain).
  - Prevents loading resources from external or malicious domains.

- ```script-src: 'self'```
  - Allows scripts to be loaded only from the same origin.
  - Blocks inline scripts and external JavaScript unless explicitly allowed.
  - Helps prevent XSS attacks by disallowing scripts injected by attackers.

- ```img-src: 'self' data:'```
  - Allows images to be loaded from the same origin ('self') and embedded Base64-encoded images (data: scheme).
  - Blocks loading images from external sources unless explicitly permitted.

```python
Talisman(app, content_security_policy={
    'default-src': "'self'",
    'script-src': "'self'",
    'img-src': "'self' data:'",
})
```

You can allow **external APIs**, **CDNs**, or **specific scripts** by modifying the CSP settings.

```python
Talisman(app, content_security_policy={
    'default-src': "'self'",
    'script-src': "'self' https://cdn.example.com'",
    'style-src': "'self' 'unsafe-inline'",  # Allows inline styles (less secure)
})

csp = {
    'default-src': "'self'",
    'script-src': "'self' https://apis.google.com",
    'style-src': "'self' 'unsafe-inline'",
    'img-src': "'self' data:'",
    'frame-ancestors': "'none'",  # Prevent clickjacking
    'object-src': "'none'",  # Prevent Flash-based attacks
}

Talisman(app, content_security_policy=csp, force_https=True, strict_transport_security=True)
```

If an attacker tries to inject a malicious script like:
```html
<script src="http://malicious.com/evil.js"></script>
```
The browser blocks it because of the CSP policy (script-src: 'self')

## Configuring Logging

```python
import logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

logger.error(f"Error fetching items: {str(e)}", exc_info=True)
```

```python
import logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

@app.route('/api/items', methods=['GET'])
@limiter.limit("10 per minute")
def get_items():
    logger.info("Received request to fetch items.")

    try:
        items_ref = db.collection('items')
        docs = items_ref.stream()
        items = [{'id': doc.id, **doc.to_dict()} for doc in docs]
        
        logger.info(f"Fetched {len(items)} items from Firestore.")
        return jsonify(items)
    
    except Exception as e:
        logger.error(f"Error fetching items: {str(e)}", exc_info=True)
        return jsonify({"error": "Failed to fetch items"}), 500
```

## Debugging & Logging

1. Logs every request:

```python
logger.info(f"Received request to delete document with ID: {doc_id}")
```

2. Validates input and logs warnings:

*If ```doc_id``` is empty, a ValueError is raised and logged as a warning.8

```python
if not doc_id:
    raise ValueError("Document ID is required for deletion")
```

3. Handles unexpected errors and logs details:
*Captures unexpected exceptions and logs a detailed error message with a full traceback ```(exc_info=True)```*

```python
except Exception as e:
    logger.error(f"Unexpected error while deleting document {doc_id}: {str(e)}", exc_info=True)
```

## Web Security Headers

| Concepts                            | Definitions                                                                 | Risk                                                   | When to Use It?                                   | Flask Code Snippet |
|-------------------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------|--------------------|
| **Content Security Policy (CSP)**   | Controls sources of content, preventing issues like cross-site scripting (XSS). | XSS attacks, data injection                             | When embedding third-party content or scripts.   | `CSP = "default-src 'self'"` |
| **X-Frame-Options**                 | Prevents clickjacking attacks by disallowing your site to be embedded in iframes. | Clickjacking attacks                                   | When your site should not be embedded elsewhere. | `X_FRAME_OPTIONS = "DENY"` |
| **X-Content-Type-Options**          | Prevents browsers from MIME-sniffing a response, ensuring content types match declared types. | MIME-type confusion, content spoofing                  | When serving files to prevent MIME-sniffing.     | `X_CONTENT_TYPE_OPTIONS = "nosniff"` |
| **Referrer-Policy**                 | Controls the amount of referrer information sent with requests.              | Leakage of sensitive referrer data                     | When controlling referrer data exposure.         | `REFERRER_POLICY = "no-referrer"` |
| **Strict-Transport-Security (STS)** | Instructs browsers to use HTTPS connections only for the domain.             | Man-in-the-middle attacks, downgrade attacks          | When enforcing HTTPS security.                   | `STRICT_TRANSPORT_SECURITY = "max-age=31536000; includeSubDomains"` |
| **Cache-Control**                   | Controls caching behavior of browsers, mitigating cache-related vulnerabilities. | Sensitive data stored in cache                         | When serving dynamic or private content.         | `CACHE_CONTROL = "no-store"` |
| **Authorization Headers**           | Used for passing authentication credentials, like JWTs or API tokens.        | Token leakage, unauthorized access                     | When securing APIs with authentication tokens.   | `Authorization: Bearer <token>` |
| **Signed Headers**                  | Cryptographically signed headers to ensure integrity and authenticity of requests. | Request tampering, replay attacks                      | When verifying authenticity of requests.         | `AWS Signature v4, JWT signature validation` |
| **Cross-Origin Resource Sharing (CORS)** | Mechanism to control which domains can access your web resources, protecting against unauthorized cross-origin requests. | Cross-site request forgery (CSRF), unauthorized access | When exposing APIs to other domains.             | `CORS(app, resources={r"/*": {"origins": "*"}})` |


```python
@app.after_request
def add_security_headers(response):
    response.headers['Strict-Transport-Security'] = 'max-age=31536000; includeSubDomains'
    response.headers['X-Content-Type-Options'] = 'nosniff'
    response.headers['X-Frame-Options'] = 'DENY'
    return response
```

## Define Pydantic Models

Must be place into ```models.py```
```python
from pydantic import BaseModel, Field
from typing import Optional

class ItemCreate(BaseModel):
    """ Pydantic model for item creation """
    name: str = Field(..., min_length=3, max_length=100, description="Item name")
    description: str = Field(..., min_length=5, max_length=500, description="Item description")

class ItemUpdate(BaseModel):
    """ Pydantic model for updating an item """
    name: Optional[str] = Field(None, min_length=3, max_length=100)
    description: Optional[str] = Field(None, min_length=5, max_length=500)

```
Must be place into ```blueprints/items.py```

```python
from flask_pydantic import validate

@items_bp.route('/api/items', methods=['POST'])
@limiter.limit("5 per minute")
@token_required
@validate()
def create_item(body: ItemCreate):
    """ Securely create an item with validated data """

    item_ref = db.collection('items').document()
    item_ref.set({
        'name': body.name,
        'description': body.description
    })
    
    return jsonify({"message": "Item created successfully"}), 201
```

```python
@items_bp.route('/api/items/<item_id>', methods=['PUT'])
@limiter.limit("5 per minute")
@token_required
@validate()
def update_item(item_id: str, body: ItemUpdate):
    """Update an existing item in Firestore."""

    item_ref = db.collection('items').document(item_id)
    item = item_ref.get()

    if not item.exists:
        return jsonify({"error": "Item not found"}), 404

    update_data = {}

    if body.name:
        update_data["name"] = body.name
    if body.description:
        update_data["description"] = body.description

    if update_data:
        item_ref.update(update_data)
        return jsonify({"message": "Item updated successfully"}), 200
    else:
        return jsonify({"error": "No valid fields to update"}), 400
```

ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif'}












