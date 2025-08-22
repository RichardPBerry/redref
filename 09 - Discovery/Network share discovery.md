# Network Share Discovery

**Side note for smbclient**
smbclient is a very useful multipurpose tool for examining SMB shares. Note that `smbclient` understands both forwards slashes `/` and back slashes `\`. However, backslashes are read as an escape character, so need to be doubled. For example, `//somehost/someshare` becomes `\\\\somehost\\someshare`.


# 1. Enumerate shares

## Enumerating shares without credentials
It can be useful to try with various types of guest/anonymous accounts to see if any shares allow unauthenticated access. NetExec is very helpful for this.
- `nxc smb $target_ip -u '' -p '' --shares`: Enumerate with empty username/password
- `nxc smb $target_ip -u 'guest' -p '' --shares`: Enumerate with guest username/password
- `nxc smb $target_ip -u 'anonymous' -p '' --shares`: Enumerate with anonymous username/password

Alternatively can use smbclient:
- `smbclient -N -L \\\\$target_ip -U $user%$pass | tee 09_smbclient_auth_list.txt` : Get a list (-L) of shares on target host, with no authentication (-N)


## Enumerating shares with credentials
- `nxc smb $target_ip -u $user -p $pass --shares | tee 09_netexec_enumerate_shares.txt`: Enumerate with anonymous username/password

Alternatively:
- `smbclient -N -L \\\\$target_ip -U | tee 09_smbclient_noauth_list.txt` : Get a list (-L) of shares on target host, with no authentication (-N)


# 2. Enumerating share content with smbmap
Once you have read access to a share, smbmap is very usefult to dump out the contents of the shares to look for files that may be of interest.
- `smbmap -H $target_ip -r -u $user -p $pass | tee 09_smbmap_filedump.txt`: Run a recursive (-r) enumeration attempt on target host (-H) with given username (-u) and password (-p)ss


# 3. Start and interactive session with smbclient
After identifying a target share start an interactive session to investigate or retrieve the files.
- `smbclient \\\\$target_ip\\[someshare] -U $user%$pass`: Attempt to connect to the specified network share using the given username and password (-U)

