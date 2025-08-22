# Password cracking

# Hash identification
First stop with any hash is to understand what you have.
- `hash-identification hashes.txt`: Generally I find this does not work very well
- [Hashes.com Hash Identifier](https://hashes.com/en/tools/hash_identifier): seems to be quite a reliable way of doing it



## NTLMv2
Used in Windows authentication.
Example:
```
dmin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
```
**Crack formats:**
- john: `format=netntlmv2`
- hashcat: `mode=5600`



# Cracking

## Setup
If not already done, install seclists:
- `sudo apt install seclists`
- `sudo gunzip /usr/share/wordlists/rockyou.txt`: rockyou comes in gzip format - need to unzip this before using it

## John the ripper
**Cracking modes:**
- `john --format=$format hashes.txt`: Default mode
- `john --format=$format hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt`: Wordlist mode using rockyou.txt



**References:**
- [John the ripper hash formats](https://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats)
- [Hascat modes](https://hashcat.net/wiki/doku.php?id=example_hashes)