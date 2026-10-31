# Example Audit Report: Client-Side Secret & Sensitive-File Exposure
scattered across **every layer**; HTML meta tags, hidden DOM elements, HTML
### 1: Critical: Admin token, JWT secret, DB password shipped to the browser
  (allowing forged tokens for any role), the API secret, and DB credentials; potential access within the credential's effective permissions if live; network
  **Rotate every value**; anything served is already compromised.
### 2: Critical: Cloud & payment provider secrets exposed
  Redis password enables cache/session tampering; if live.
### 3: High: Server-side source, config & deploy files publicly reachable
  names, and embedded credentials; a blueprint for further attack.
### 4: High: reCAPTCHA secret key in an HTML meta tag
### 5: Medium: Missing security headers
  (`Strict-Transport-Security` **is** present; good.)
### 6: Medium: Permissive CORS
### 7: Medium: Insecure randomness for booking references
- **Location:** `src/services/booking.service.js`; `randomRef()` uses
### 8: Low/Info: Third-party tokens & noisy logging