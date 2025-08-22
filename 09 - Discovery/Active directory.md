# Active Directory Discovery

# Enumerating Users
Can use NetExec to enumerate a bunch of information:
This command will enumerate domain groups, local groups, logged on users, relative identifiers (RIDs), sessions, domain users, SMB shares/permissions, and get the domain password policy. You can also use CIDR notation to target a range of ip addresses (i.e. 10.10.10.0/24). From [WADComs GitHub](https://wadcoms.github.io/wadcoms/NetExec-Enum-SMB/).

`nxc smb $target_ip -u "$user" -p "$pass" --groups --local-groups --loggedon-users --rid-brute --users --shares --pass-pol | tee 09_netexec_enumerate_all.txt`


# Domain dumping
Another good option is to dump the entire domain
`ldapdomaindump ldap://$target_ip -u "$domain\\$user" -p $pass`



# AD object modification
## Modifying group membership
Add user $target_user to group $target_group using bloodyAD:
`bloodyAD --host $target_ip -d $domain -u $user -p $pass  add groupMember $target_group $target_user$` 


# Bloodhound
So good. So, so good.

## Starting bloodound
1. Follow the instructions at [Kali Bloodhound](https://www.kali.org/tools/bloodhound/) to install and setup.
2. Once configured, start with `sudo bloodhound`.
3. This will open the web GUI


## Data collectors
The official bloodhound collectors are for Windows platforms. However, bloodhound-python is natively built into kali and can do this. The command below will collect all data from the domain and compress to a zip file. Note that [DC Name] should the be the name of a domain controller (e.g. DC01.fluffy.htb).

`bloodhound-python -u '$user' -p '$pass'  -d $domain -ns $target_ip -dc [DC Name] -c All -gc $domain --zip`

*Troubleshooting*
1. In some cases it may be necessary to run a time sync to the target domain in order for certificates to be valid: `sudo ntpdate $target_ip`
2. It may also be helpful to update `/etc/resolve.conf` to add a nameserver for the target IP so that name resolution works correctly. Using the `-ns` flag with bloodhound-python should make this unnecessary.
```
search mshome.net 
#nameserver 172.18.16.1 
nameserver 10.10.11.69 
```

## Using Bloodhound GUI
- Upload the data to bloodhound from the GUI by going to Administration > File Ingest > Upload File(s). 
- Once uploaded go to the Explore tab > Search Nodes > Type in the name of a user or group
- Also try out pathfinding from your current position to the target node. Select the arrows between nodes to see if an escalation path is possible