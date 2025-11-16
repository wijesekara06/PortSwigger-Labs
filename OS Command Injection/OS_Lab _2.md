2. Blind OS command injection with time delays

SOLUTION

STEP 01: Open the Lab and go to the "Submit feedback" page.

STEP 02: Fill in the form and intercept the POST request using Burp Suite. Send the request to Burp Repeater.

STEP 03: This is a blind vulnerability, so we won't see any output. We must test for it by injecting a command that causes a time delay, such as sleep. We test various parameters. Injecting in the name field has no effect.

STEP 04: Now we test the email parameter. We inject the payload & sleep 10 #. This is URL-encoded to %26+sleep+10+%23.

Payload: ...&email=test%40gmail.co%26+sleep+10+%23&subject=...
(This decodes to: ...&email=test@gmail.co& sleep 10 #&subject=...)

STEP 05: Upon sending the request, the server hangs for 10 seconds before responding. This confirms the sleep 10 command was executed. The lab is now solved.