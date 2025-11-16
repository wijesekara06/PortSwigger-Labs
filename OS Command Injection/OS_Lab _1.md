1. OS command injection, simple case

SOLUTION

STEP 01: Open the Lab. Intercept a "Check stock" request and send it to Burp Repeater. The request is a POST to /product/stock and sends productId and storeId parameters.

STEP 02: To test for OS command injection, we inject a shell command. We use the & metacharacter to append a new command.

Payload 1: productId=1 & whoami&storeId=1

STEP 03: This payload causes an error. The response shows "unbound variable" and "whoami: extra operand '1'". This is a good sign! It means the whoami command did execute, but it tried to interpret the rest of the string (&storeId=1) as an argument, which failed.

STEP 04: To fix this, we need to comment out the rest of the command so it's ignored by the shell. We can use the # character (URL-encoded as %23).

Payload 2 (Final): productId=1+%26+whoami+%23&storeId=1
(This decodes to: 1 & whoami #)

STEP 05: Send the new payload. The server responds with the output of the whoami command, which is the current username (peter-folelt).

STEP 06: The lab is now solved.