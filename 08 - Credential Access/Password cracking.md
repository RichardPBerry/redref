# Password cracking
**NOTE:** Do NOT post real hash values here!!


# Hash identification
First stop with any hash is to understand what you have.
- `hash-identifier hashes.txt`: Generally I find this does not work very well
- [Hashes.com Hash Identifier](https://hashes.com/en/tools/hash_identifier): seems to be quite a reliable way of doing it
- `hashcat hashes.txt`: This will run hashcat in automode, which does a decent job of trying to ID the hash


# Formats
The following provides a quick guide to various common hash formats:

| Format    | Comments              | John format   | Hashcat mode  | Example |
| NTLMv2    | Used in Windows auth  | netntlmv2     | 5600          | admin:N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030|
| md5       | Legacy, oudated       | raw-md5       | 0             | 8743b52063cd84097a65d1633f5c74f5 |
|sha256     | Common in web apps    | raw-sha256    | 1400          |  |
| Salted sha256 |                   | ???       | 1410 / 1420   | c73d08de890479518ed60cf670d17faa26a4a71f995c1dcc978165399401a6c4:53743528 |


# Cracking

## Seclists
Selists provides and pre-built library of common wordlists and can be easily installed with `sudo apt install seclists`.

## Rockyou
Kali comes pre-loaded with rockyou - a large and very commonly used wordlists. However, it needs to be unpacked before use with `sudo gunzip /usr/share/wordlists/rockyou.txt`. The word lists is the located under:
```
/usr/share/wordlists/rockyou.txt
```

## Hashcat
Hashcat is typically the first choice of ripping tool. The general usage is:
```
hashcat -a [attack mode] -m [mode/format] hash_file [dictionary file]
```c73d08de890479518ed60cf670d17faa26a4a71f995c1dcc978165399401a6c4

**Example usage:** 
```
# Default mode - useful for identifing hashes
hashcat hashes.txt

# Set the format: https://hashcat.net/wiki/doku.php?id=example_hashes
mode=1400       # sha256

# Crack in default attack mode
hashcat -m $mode hashes.txt

# Use a wordlists
hashcat -m $mode hashes.txt /usr/share/wordlists/rockyou.txt
```

## John the ripper
While older, John is a solid ripping tool and sometimes easier to use than hashcat. It can be hard to find the right format for John.

Note that for John hash values can be preceeded with `username:`, which can be helpful, e.g:

```
cat hashes.txt
# md5
admin:8743b52063cd84097a65d1633f5c74f5

# NTLMv2
admin:N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
```

John the ripper formats can be found here: [John the ripper hash formats](

Note: If you are getting the 'No password hashes left to crack' message, remove the .pot file (where John stores previously cracked hashes): `rm ~/.john/john.pot`.

**Example usage:**
```
# Set the format: https://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats)
format='raw-sha256'

# Default mode, will attempt to automatically crack
john --format=$format hashes.txt

# Wordlist mode
john --format=$format hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```