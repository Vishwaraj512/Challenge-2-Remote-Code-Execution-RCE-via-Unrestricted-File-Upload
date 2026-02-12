# Challenge-2-Remote-Code-Execution-RCE-via-Unrestricted-File-Upload

1. Finding Overview
Vulnerability Name: Unrestricted File Upload

Severity: Critical

Status: Exploited (RCE achieved)

2. Methodology (Manual Process)
Reconnaissance: I identified a file upload feature at /posts/upload-article.php designed for article submissions. I noted the server was running PHP 7.4.33 by inspecting the response headers in Burp Suite.

Initial Testing: I manually submitted a standard .txt file named qFlipper... and observed the multipart POST request in the Proxy -> HTTP history tab.

Exploitation:

I created a PHP web shell (exploit.php) containing a system command execution payload: <?php system($_GET['cmd']); ?>.

Using Burp Suite Intercept, I captured the upload request.

I manually bypassed server-side filters by modifying the Content-Type: text/plain header to Content-Type: image/png while keeping the .php extension.

Command Execution: Once the upload was accepted, I navigated to the uploaded file location and used the cmd parameter to execute the ls and cat commands to retrieve the flag.

3. Evidence
Submission Form: [Insert image_097623.png]

Intercepted Malicious Request: [Insert image_0abc5c.jpg showing the filename and content-type]

Captured Flag: [Insert your screenshot of the browser showing the FLAG{...} result]

4. Remediation
Extension Whitelisting: The server should only allow specific, non-executable extensions (e.g., .pdf, .docx).

Content Validation: Implement server-side "Magic Byte" checking to verify the actual file type, rather than trusting the Content-Type header.

Execution Prevention: The /uploads/ directory should be configured to disable script execution (e.g., using .htaccess to prevent PHP from running in that folder).
