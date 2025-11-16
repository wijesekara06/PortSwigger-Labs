# 🧠 OS Command Injection: PortSwigger Labs Walkthrough

OS Command Injection is a **web security vulnerability** that allows an attacker to execute arbitrary commands on the **operating system** of the server running an application.  
This vulnerability occurs when **unsanitized user input** (like form inputs, cookies, or HTTP headers) is passed directly to a **system shell**.

---

## ⚙️ How Does It Work?

Attackers exploit this flaw by injecting **OS command metacharacters** into user inputs.  
These characters terminate the original command and allow the execution of new, malicious commands.

### Common Payload Characters

| Character | Description |
|------------|--------------|
| `&` | Executes the injected command **after** the original command finishes. |
| `|` | Pipes the output of the original command into the injected command. |
| `;` | Separates commands; executes them sequentially. |
| `||` | Executes the injected command **only if** the original command fails. |
| `&&` | Executes the injected command **only if** the original command succeeds. |
| `$(...)` or `` `...` `` | Command substitution — inserts output of one command into another. |

---

## 🧩 Types of OS Command Injection

### 1. Direct (In-band)
The output of the injected command appears directly in the application’s HTTP response.  
✅ **Easiest to exploit**, as the attacker gets immediate feedback.

### 2. Blind (Out-of-band)
The output is **not** returned directly.  
Attackers confirm success using indirect methods such as:

- **Time Delays:** Commands like `sleep` or `ping` delay responses to prove code execution.
- **Output Redirection:** Redirecting command output to a file in a writable web directory.
- **Out-of-Band (OOB):** Using tools like `curl` or `nslookup` to exfiltrate data externally.

---
