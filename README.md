# REDREF

A red teaming reference for attacking boxes. This repository contains notes, comands and tips for penetration testing. Notes are arranged according to the [MITRE ATT&CK framework](https://attack.mitre.org/), which also aligns nicely with the way Kali Linux is structured.

Note that this repository is NOT intended to be a guide to pen testing or attacking machines. Instead, it is intended to be a concise reference of commands and tips that are useful for pentesting.

## Quickstart
Copy the `00 - template` folder and rename the folder with the name of your project. This folder will store all scripts and outputs specific to the project.

The template contains the following files and structure:

```
[myproject]
    |
    |__ [myproject_notes].md    <-- Markdown file for storing notes, testing ideas and outcomes, etc.
    |__ requirements.txt        <-- Python libraries used within this project
    |__ variables.sh            <-- Stores variables for the target environment. `source variables.sh` to import into the current shell session.
    |__ .gitignore              <-- Tell git to ignore certain files/filetypes if using version control
    |__ .venv               <-- Folder for python interpreter specific to this project (see below)
```

### Python virutal environments
When crafting payloads or exploits or running custom cracking tools it is often useful to use Python. The best way of doing this is to create a virtual environment within the project folder.
- Install venv: `sudo apt install python3-venv`

Then from the root project folder run the following:

```
python -m venv .venv        # Create a new virtual environment called .venv in the current directory
source .venv/bin/activate   # Activate the environment
touch requirements.txt      # Create a requirements.txt file
```

By convention, the filename `requirements.txt` signifiies the python  libraries required for the project (one library per line). 
Install required packages listed in requriements.txt by using `pip install -r requirements.txt`


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

