Overview — OS Command Injection

OS Command Injection is a critical web application vulnerability where an attacker is able to inject and execute arbitrary operating-system level commands on the server that runs the web application. It occurs when user-supplied input is incorporated into shell commands without proper validation or sanitization, allowing the attacker to break out of the intended command context.

Why it matters

Severity: Can lead to full server compromise, data exfiltration, privilege escalation, or pivoting to other internal systems.

Impact examples: read or modify files, create web-accessible backdoors, run processes, add new users, or exfiltrate secrets.

Prevalence: Often introduced by insecure use of system calls (system(), exec(), popen(), backticks, or shell invocations from web code).

How it typically works (high level)

Application builds a shell command using user input (e.g., sh script.sh --user $input).

Attacker injects shell metacharacters or constructs (&, ;, |, $(...), backticks) into the input.

Shell interprets those characters and executes attacker-supplied commands, possibly returning output directly or indirectly.

Common exploitation techniques

Direct (in-band): Inject command and read output in the HTTP response (easy to confirm).

Time-based (blind): Use sleep/ping to create observable delays when output is not returned.

File-based (blind): Redirect command output to a writable, web-accessible path and then retrieve it.

OOB (out-of-band): Use curl, nslookup, or ping to send data to an attacker-controlled host.

Typical indicators / testing hints

Injection works when special characters are reflected in server errors or cause unusual responses.

Unexpected delays after requests (time-based tests) indicate a blind injection.

Writable web directories (e.g., /var/www/…) allow file-based exfiltration.

Example short payloads

& whoami # — run whoami and comment out rest of command.

; ls -la /tmp ; — list files (if ; is accepted).

& sleep 10 # — cause a 10-second delay (blind test).

& whoami >/var/www/images/out.txt # — redirect output to a web-accessible file.

Always URL-encode characters when testing through HTTP parameters (e.g., # → %23, & → %26, > → %3E).

How to prevent it (best practices)

Avoid shelling out: Don’t construct shell commands with user input. Use native APIs or libraries instead.

Input validation & allowlists: Validate inputs strictly (type checks, regex, allowlists), and reject suspicious characters/constructs.

Proper escaping: If you must call shell commands, use safe APIs that avoid the shell (e.g., execve style with argument arrays) or robust escaping libraries.

Least privilege: Run services with minimal privileges and limit what file paths and commands the service user can access.

Containment: Use chroot, containers, AppArmor/SELinux policies, or seccomp to reduce the blast radius.

Logging & monitoring: Detect anomalies (unexpected commands, delays, new files in web directories, outbound DNS/HTTP requests).

Code review & secrets management: Audit all places that build commands from input and keep secrets out of code.

Detection & tooling

Manual testing tools: Burp Suite (Repeater, Intruder), OWASP ZAP.

Automated scanners: Some SAST/DAST solutions detect risky uses of shell calls.

Runtime monitoring: EDR/host monitoring to detect suspicious process creation or network callbacks.

