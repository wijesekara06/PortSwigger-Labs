3. Blind OS command injection with output redirection

SOLUTION

This is another blind OS command injection. We cannot see the command's output directly, but the lab tells us there is a writable directory at /var/www/images/. We can redirect our command's output to a file in that directory and then browse to the file to read it.

STEP 01: Go to the "Submit feedback" page, fill out the form, and intercept the POST request. Send it to Burp Repeater.

STEP 02: First, confirm the vulnerability using the time-based payload from the previous lab.

Payload: ...&email=test%40gmail.co%26+sleep+10+%23&...
(Decodes to: ...&email=test@gmail.co& sleep 10 #&...)

The server will hang for 10 seconds, confirming the email parameter is vulnerable.

STEP 03: Now, execute the whoami command and redirect its output (>) to a file named output.txt inside the writable directory /var/www/images/.

Payload: ...&email=test%40gmail.co%26+whoami+>/var/www/images/output.txt+%23&...
(Decodes to: ...&email=test@gmail.co& whoami > /var/www/images/output.txt #&...)

Send this request. The response will be fast, as the command execution is asynchronous.

STEP 04: To read the file, we need its URL. Go to your Burp "HTTP history" and find any GET request for an image (e.g., /image?filename=10.jpg). Send this request to Repeater.

STEP 05: Modify the GET request. Change the filename parameter from the original image to output.txt.

Request: GET /image?filename=output.txt HTTP/2

STEP 06: Send the request. The server will respond with the contents of your file, which is the output of the whoami command: peter-Musk2p.

This solves the lab.