🔐 PicoCTF – Crack the Gate 1 (Writeup)

🧠 Overview

Ek restricted login portal diya tha jahan:

- Email known tha: "ctf-player@picoctf.org"
- Password unknown tha
- Normal login fail ho raha tha

Hint mila: developer ne secret way chhoda hai

---

🔍 Analysis

1. Login page open kiya
2. Inspect Element ("F12") use kiya
3. HTML me ek hidden encoded message mila:

ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"

---

🔓 Decoding

ROT13 se decode kiya:

NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"

👉 Yahan se clear ho gaya:

- Ek hidden developer backdoor exist karta hai

---

⚡ Exploitation

1. Login request ko intercept kiya (DevTools / Burp)
2. Request me custom header add kiya:

X-Dev-Access: yes

3. Modified request server ko send kiya

---

🏁 Result

- Authentication bypass ho gaya ✅
- Password ki zarurat nahi padi ❌
- Flag mil gaya 🎯

---

💀 Vulnerability

- Broken Access Control
- Server client-side header pe trust kar raha tha

---

🧠 Real-Life Lesson

- Kabhi bhi client input (headers, params) pe trust nahi karna chahiye
- Dev/debug backdoors production me nahi rehne chahiye

---

🧭 Takeaway

Inspect → Decode → Modify → Exploit
