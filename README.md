# XSS Demo

This repository contains a small HTML demo used to illustrate how unsafe handling of user-supplied input can lead to cross-site scripting (XSS) risks.

**What this repo contains**

- `index.html` — a tiny page that reads a `name` query parameter and shows it in the page; it also demonstrates setting a cookie via `document.cookie`.

**Security notes & recommendations**

- Vulnerability: inserting untrusted input into the DOM using `innerHTML` (or otherwise injecting raw HTML) allows script execution and XSS.
- Avoid this by using `textContent` or `createTextNode` and DOM APIs so input is treated as text, not HTML.
- For production systems, perform server-side input validation and sanitization and use a vetted sanitizer (for example, DOMPurify) if any HTML must be allowed.
- Set sensitive cookies from the server with the `HttpOnly` and `Secure` flags so they are not readable from JavaScript.
- Use a Content Security Policy (CSP) to reduce the impact of possible injections.

**How to run**

Serve the folder and open `index.html` in a browser. For a quick local server:

```sh
python -m http.server 8000
# then open http://localhost:8000/
```
