# Daily Flask Application


- 👉 [Purpose](Web Security Headers)
- 👉 CORS
- 👉 BluePrint
- 👉 Rate Limiting
- 👉 Session Security
- 👉 Logging
- 👉 Async - I/O
- 👉 Pydantic
- 👉 Try / except
- 👉 except requests.exceptions.RequestException as e:

- 👉 Role-Based Access Control (RBAC) – Restrict actions based on user roles.
- 👉 Multi-Factor Authentication (MFA) – Enforce additional authentication layers.



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
