Absolutely 😊
Here is a **complete interview-ready CSP + Trusted Types + XSS Cheat Sheet** for a **5-year API/Web Pentester**.

---

# ✅ CSP + Trusted Types Cheat Sheet (Pentest Interview)

---

# 1️⃣ What is CSP?

### Definition (Interview One-Liner)

> Content Security Policy (CSP) is a browser-enforced security header that restricts where scripts, styles, images, frames, and other resources can load from, mainly to mitigate XSS.

---

# 2️⃣ Most Important CSP Directives

| Directive         | Controls                            |
| ----------------- | ----------------------------------- |
| `default-src`     | Baseline fallback for all resources |
| `script-src`      | JavaScript execution sources        |
| `style-src`       | CSS sources                         |
| `img-src`         | Image sources                       |
| `connect-src`     | AJAX/fetch/WebSocket calls          |
| `frame-src`       | iframe content                      |
| `frame-ancestors` | Clickjacking protection             |
| `object-src`      | Flash/embed/object (should be none) |
| `base-uri`        | Prevent `<base>` tag injection      |

---

# 3️⃣ Safe Baseline CSP (Best Practice)

```http
Content-Security-Policy:
default-src 'none';
script-src 'nonce-random' 'strict-dynamic';
object-src 'none';
base-uri 'none';
frame-ancestors 'none';
```

✅ Very hard to bypass
✅ Best modern standard

---

# 4️⃣ Weak / Vulnerable CSP Configurations

---

## ❌ `unsafe-inline`

```http
script-src 'unsafe-inline'
```

### Why vulnerable?

Allows inline scripts + event handlers:

```html
<img onerror=alert(1)>
```

---

## ❌ `unsafe-eval`

```http
script-src 'unsafe-eval'
```

### Why vulnerable?

Allows:

* `eval()`
* `new Function()`

Attackers can execute injected JS strings.

---

## ❌ Wildcards

```http
script-src *
```

### Why vulnerable?

Any attacker domain can host malicious JS.

---

## ❌ Over-trusting CDNs

```http
script-src https://cdnjs.cloudflare.com
```

### Risk:

* Gadget chains
* Supply chain compromise

---

## ❌ Missing `object-src none`

If not set, legacy plugin vectors may exist.

---

## ❌ Missing `base-uri none`

Allows `<base href>` injection → redirect script loading.

---

# 5️⃣ CSP Bypass Techniques (Modern)

---

## ✅ 1. DOM XSS Gadgets (Most Common Today)

Even with strong CSP:

* Attacker exploits existing trusted JS sinks:

```js
element.innerHTML = userInput;
```

This is called:

> CSP Gadget-Based XSS

---

## ✅ 2. JSONP Endpoints on Allowed Domains

Whitelisted domain provides:

```html
<script src="trusted.com/api?callback=alert(1)">
```

---

## ✅ 3. Nonce Reuse Bug

If nonce is static:

```http
script-src 'nonce-123'
```

Attacker can reuse it → bypass.

Nonce must be random per response.

---

# 6️⃣ Nonce Explained

### Header:

```http
script-src 'nonce-x7Klm9'
```

### HTML:

```html
<script nonce="x7Klm9">
  trusted code
</script>
```

✅ Only scripts with matching nonce run
❌ Injected scripts fail (no nonce)

---

# 7️⃣ CSP vs DOM XSS (Key Interview Point)

> CSP is mitigation, not a patch.

DOM XSS still happens because vulnerable JS executes attacker input inside trusted scripts.

Fix requires:

* Safe sinks
* Sanitization
* Trusted Types

---

# 8️⃣ Trusted Types (Latest Defense)

---

## Directive:

```http
Content-Security-Policy: require-trusted-types-for 'script';
```

### Meaning:

Browser blocks dangerous DOM sinks unless they receive a **Trusted Type object**, not a raw string.

---

## Blocks sinks like:

* `innerHTML`
* `outerHTML`
* `document.write`
* `eval`

---

## Example:

❌ Blocked:

```js
div.innerHTML = userInput;
```

✅ Allowed only through policy:

```js
div.innerHTML = policy.createHTML(userInput);
```

---

# 9️⃣ DOM XSS Patch Cheat Sheet

---

## Dangerous sinks (Avoid)

```js
innerHTML
outerHTML
document.write()
eval()
setTimeout("string")
```

---

## Safe alternatives

```js
textContent
innerText
createElement()
DOMPurify.sanitize()
```

---

# 🔟 Strongest Modern XSS Defense Stack

✅ Fix DOM sinks
✅ Nonce-based CSP
✅ strict-dynamic
✅ Trusted Types
✅ Output encoding + sanitization

Example:

```http
Content-Security-Policy:
default-src 'none';
script-src 'nonce-random' 'strict-dynamic';
object-src 'none';
base-uri 'none';
require-trusted-types-for 'script';
frame-ancestors 'none';
```

---

# ⭐ Interview Quick Answers

---

### Q: Weakest CSP directive?

✅ `'unsafe-inline'`

---

### Q: Most secure CSP approach?

✅ Nonce + strict-dynamic + default-src none

---

### Q: Latest bypass trend?

✅ DOM gadget-based XSS

---

### Q: Latest mitigation?

✅ Trusted Types

---

### Q: CSP stops DOM XSS fully?

❌ No, only reduces impact.

---

# ✅ Final Interview One-Liner

> “Modern CSP security requires nonce-based policies with strict-dynamic, and Trusted Types to prevent DOM XSS gadget bypasses. Unsafe-inline, eval, wildcards, and broad CDNs are common weak points.”

---

If you want, I can create the next cheat sheet on:

✅ SSRF → Cloud takeover
✅ OAuth2/OIDC attack chains
✅ OWASP API Top 10 (real payload mindset)
✅ JWT vulnerabilities

Just tell me which one.
