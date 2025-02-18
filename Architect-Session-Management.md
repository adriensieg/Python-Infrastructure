# Session Management

A session is a way to **maintain state** and **user-specific information** across **multiple HTTP requests** in a web application.

## Purpose & Goals:

- **Maintain user state** since HTTP is **stateless** by nature
- **Store user-specific** information **temporarily**
- Enable **authenticated user experiences**
- **Preserve data** between page loads/requests
- Improve security by **avoiding constant re-authentication**

## Information typically stored in sessions:

1. User **authentication status** and **user ID**
2. **User preferences** and **settings**
3. **Shopping cart contents**
4. **Form wizard progress**
5. **CSRF tokens** for security
6. **Temporary data** needed across requests

## Session vs. No Session:
Without sessions, you would need to:

Send all state information with every request
Re-authenticate on every page load
Rely heavily on client-side storage (cookies/localStorage)
Handle more complex state management
Deal with increased security risks

Technical Concepts:

Session ID


Unique identifier generated for each session
Usually stored in a cookie or URL parameter
Should be cryptographically secure random string


Session Storage Methods:
