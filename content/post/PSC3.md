+++
author = "Hugo Authors"
title = "ProLUG SEC Unit 3 🔒"
date = "2025-04-13"
description = "This is a Template for Reporting on ProLUG Courses"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "CapstoneProject"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

## Intro 👋

Understanding and implementing network standards and compliance measures can make security controls of critical importance very effective.[^1]

---

## Worksheet

### `Discussion Post 1`

There are 16 Stigs that involve PAM for RHEL 9[^2].

`Question`

- What are the mechanisms and how do they affect PAM functionality?

`Answer`

#### Hardening Defaults
STIGs replace permissive PAM modules with stricter ones. 2 Categories/Areas are covered in regards to Stig'ing PAMs

1. Lockout Policies that effect login frequency and failure.
2. Password Strength Enforcement that effects password complexity and re-use.

#### Review /etc/pam.d/sshd on a Linux system.

`Question`

- What is happening in that file relative to these functionalities?

`Answer`

- This file specifies the PAM module control flags that sshd uses during authentication. 

`Question`

 - What are the common PAM modules?

`Answer`

- pam_sepermit.so, pam_nologin.so, apassword-auth and postlogin.

`Question`

- Look for a blog post or article about PAM that discusses real world application. ost it here and give us a quick synopsis.

`Answer`

https://www.redhat.com/en/blog/pluggable-authentication-modules-pam?utm_source=chatgpt.com

`Synopsis:`

PAM are a modular and flexible framework for integrating authentication methods into applications. By seperating / abstracting authentication mechanisms from application code, PAM allows admins to manage authentication policies centrally. PAM also allows from customized authentication processes (Security through obscurity)

### `Discussion Post 2`

Intro to the scenario

Read about active directory (or LDAP) configurations of Linux via sssd[^3] 👍

`Question`

- Why do we not want to just use local authentication in Linux? Or really any system?

`Answer`

- Local authentication presents several problems. Firstly, there is no federated access, so there is fragmentation of systems. Secondly, scalability is an issue as each local system manages that local system's users, requiring individual account provisioning and password management. Thirdly, it complicates auditing and compliance, since there is no centralized logging or consistent policy enforcement. Additionally, stale or orphaned accounts can accumulate unnoticed, increasing security risks. Finally, it prevents the implementation of modern security practices such as single sign-on (SSO), multi-factor authentication (MFA), and role-based access control across a distributed environment.

`Question`

- There are 4 SSSD STIGS.

`Response`

#### Vuln ID 258122

**Enforce Smart Card Authentication** – Require certificate-based smart card login to implement multi-factor authentication and enhance access security.

#### Vuln ID 248131

**Validate Certificate Chains** – Ensure that certificates used for PKI-based authentication are properly validated by building a complete certification path to a trusted root.

#### Vuln ID 258132

**Associate Certificates with User Accounts** – Confirm that every authentication certificate is explicitly mapped to a valid user account to maintain identity integrity.

#### Vuln ID 258133

**Restrict Credential Caching Duration** – Limit the validity period of cached authentication credentials to a maximum of 24 hours to reduce risk in the event of compromise.

---

### Definitions

- `PAM` Pluggable Authentication Modules provide a flexible mechanism for authenticating users on Unix-like systems.
- `AD` Active Directory is Microsoft's centralized directory service for authentication, authorization, and resource management.
- `LDAP` Lightweight Directory Access Protocol is an open, vendor-neutral protocol for accessing and maintaining distributed directory information services.
- `sssd` System Security Services Daemon provides access to remote identity and authentication providers like LDAP or Kerberos.
- `oddjob` A D-Bus service used to perform privileged tasks on behalf of unprivileged users, often for domain enrollment or home directory creation.
- `krb5` Kerberos 5 is a network authentication protocol that uses tickets for securely proving identity over untrusted networks.
- `realm/realmd` A tool that simplifies joining and managing a system in a domain like Active Directory or IPA using standard services.
- `wheel` (system group in RHEL):** A special administrative group whose members are allowed to execute privileged commands using `sudo`.


## Lab

#### Examine STIG V-257986

`Question`

- What is the problem?

`Answer`

- RHEL 9 needs PAM enabled for SSHD

`Question`

- What is the fix?

`Answer`

- Enabling `UsePAM` in /etc/ssh/sshd/config

`Question`

- What type of control is being implemented?

`Answer`

- A Technical Preventative control

`Question`

- Is it set properly on your system?

`Answer`

- Yes it is

```bash
grep -i pam /etc/ssh/sshd_config
```

`Question`

- Can you remediate this finding?

#### Check and remediate STIG V-258055

`Questions`

- What is the problem?
- What is the fix?
- What type of control is being implemented?
- Are there any major implications to think about with this change on your system? Why or why not?
- Is it set properly on your system?
- How would you go about remediating this on your system?

`Answers`

- After 3 Unsuccessful root login attempts the root is locked.
- Enable faillock in authselect + `even_deny_root`
- Technical preventative
- Yes, Anyone can be locked out including root
- No, it is commented out by default `for good reason`
- I would not enable `even_deny_root`

#### Check and remediate STIG V-258098

`Questions`

- What is the problem?
- What is the fix?
- What type of control is being implemented?
- Is it set properly on your system?

`Answers`

- Password complexity module `pwquality` must be enabled in `system-auth`
- Check `/etc/pam.d/system-auth and see if the line exists or not
- Technical preventative control
- Yes it is properly implemented

#### Filter STIGS by “password complexity”

`Questions`

- How many are there?
- What are the password complexity rules?
- Are there any you haven’t seen before?

`Answers`

- 14 STIGS related to Password complexity
- The somewhat standard 4 char class (One Upper, One Lower, One Special) and 15 Char total minimum. Max repeated char class is 4 and Max repeat char 3.
- Yes the Max repeat stuff.

#### OpenLDAP Setup

You will likely not build an LDAP server in a real world environment. We are doing it for understanding and ability to complete the lab. In a normal corporate environment this is likely Active Directory.

To simplify some of the typing in this lab, there is a file located at /lab_work/identity_and_access_management.tar.gz that you can pull down to your system with the correct .ldif files.

```bash
[root@hammer1 ~]# cp /lab_work/identity_and_access_management.tar.gz .
[root@hammer1 ~]# tar -xzvf identity_and_access_management.tar 
```

##### 1. Stop the warewulf client
```bash
[root@hammer1 ~]# systemctl stop wwclient
```

##### 2. Edit your /etc/hosts file
Look for and edit the line that has your current server
```bash
[root@hammer1 ~]# vi /etc/hosts
```

Entry for hammer1 for example:
```bash
192.168.200.151 hammer1 hammer1-default ldap.prolug.lan ldap
```

##### 3. Setup dnf repo
```bash
[root@hammer1 ~]# dnf config-manager --set-enabled plus
[root@hammer1 ~]# dnf repolist
[root@hammer1 ~]# dnf -y install openldap-servers openldap-clients openldap
```

##### 4. Start slapd systemctl
```bash
[root@hammer1 ~]# systemctl start slapd
[root@hammer1 ~]# ss -ntulp | grep slapd
```

##### 5. Allow ldap through the firewall
```bash
[root@hammer1 ~]# firewall-cmd --add-service={ldap,ldaps} --permanent
[root@hammer1 ~]# firewall-cmd --reload
[root@hammer1 ~]# firewall-cmd --list-all
```

##### 6. Generate a password (Our example uses testpassword) This will return a salted SSHA password. Save this password and stalted hash for later input

```bash
[root@hammer1 ~]# slappasswd
```

Output:

`New password:  
Re-enter new password:  
{SSHA}wpRvODvIC/EPYf2GqHUlQMDdsFIW5yig`

##### 7. Change the password

```bash
[root@hammer1 ~]# vi changerootpass.ldif
```

```yaml
dn: olcDatabase={0}config,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}vKobSZO1HDGxp2OElzli/xfAzY4jSDMZ
```

```bash
[root@hammer1 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f changerootpass.ldif 
```

Output:

`SASL/EXTERNAL authentication started`
`SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth`  
`SASL SSF: 0`  
`modifying entry "olcDatabase={0}config,cn=config"`

##### 8. Generate basic schemas

```bash
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```

##### 9. Set up the domain (USE THE PASSWORD YOU GENERATED EARLIER)

```bash
[root@hammer1 ~]# vi setdomain.ldif
```

```yaml
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
read by dn.base="cn=Manager,dc=prolug,dc=lan" read by * none

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}s4x6uAxcAPZN/4e3pGnU7UEIiADY0/Ob

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
dn="cn=Manager,dc=prolug,dc=lan" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=prolug,dc=lan" write by * read
```

##### 10. Run it

```bash
[root@hammer1 ~]# ldapmodify -Y EXTERNAL -H ldapi:/// -f setdomain.ldif
```

Output:

`SASL/EXTERNAL authentication started`  
`SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth`  
`SASL SSF: 0`  
`modifying entry "olcDatabase={1}monitor,cn=config`  
`modifying entry "olcDatabase={2}mdb,cn=config`  
`modifying entry "olcDatabase={2}mdb,cn=config`  
`modifying entry "olcDatabase={2}mdb,cn=config`  
`modifying entry "olcDatabase={2}mdb,cn=config`

</blockquote>

##### 11. Search and verify the domain is working.

```bash
[root@hammer1 ~]# ldapsearch -H ldap:// -x -s base -b "" -LLL "namingContexts"
```

Output:

`dn:`
`namingContexts: dc=prolug,dc=lan`

##### 12. Add the base group and organization.

```bash
[root@hammer1 ~]# vi addou.ldif
```

```yaml
dn: dc=prolug,dc=lan
objectClass: top
objectClass: dcObject
objectclass: organization
o: My prolug Organisation
dc: prolug

dn: cn=Manager,dc=prolug,dc=lan
objectClass: organizationalRole
cn: Manager
description: OpenLDAP Manager

dn: ou=People,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: Group
```

```bash
[root@hammer1 ~]# ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f addou.ldif
```

##### 13. Verifying

```bash
[root@hammer1 ~]# ldapsearch -H ldap:// -x -s base -b "" -LLL "+"  
[root@hammer1 ~]# ldapsearch -x -b "dc=prolug,dc=lan" ou
```

##### 14. Add a user

Generate a password  (use testuser1234)

```bash
[root@hammer1 ~]# slappasswd 
```

```bash
[root@hammer1 ~]# vi adduser.ldif
```

```yaml
dn: uid=testuser,ou=People,dc=prolug,dc=lan
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: testuser
sn: temp
userPassword: {SSHA}yb6e0ICSdlZaMef3zizvysEzXRGozQOK
loginShell: /bin/bash
uidNumber: 15000
gidNumber: 15000
homeDirectory: /home/testuser
shadowLastChange: 0
shadowMax: 0
shadowWarning: 0

dn: cn=testuser,ou=Group,dc=prolug,dc=lan
objectClass: posixGroup
cn: testuser
gidNumber: 15000
memberUid: testuser
```

```bash
[root@hammer1 ~]# ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f adduser.ldif
```


##### 16. Verify that your user is in the system.

```bash
[root@hammer1 ~]# ldapsearch -x -b "ou=People,dc=prolug,dc=lan"
```

##### 17. Secure the system with TLS (accept all defaults)

```bash
[root@hammer1 ~]# openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/ldapserver.key -out /etc/pki/tls/ldapserver.crt
[root@hammer1 ~]# chown ldap:ldap /etc/pki/tls/{ldapserver.crt,ldapserver.key}
```

```bash
[root@hammer1 ~]# ls -l /etc/pki/tls/ldap*
```

Output:

`-rw-r--r--. 1 ldap ldap 1224 Apr 12 18:23 /etc/pki/tls/ldapserver.crt ` 
`-rw-------. 1 ldap ldap 1704 Apr 12 18:22 /etc/pki/tls/ldapserver.key`


```bash
[root@hammer1 ~]# vi tls.ldif
```

```yaml
dn: cn=config
changetype: modify
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/pki/tls/ldapserver.crt

add: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/pki/tls/ldapserver.key

add: olcTLSCertificateFile
olcTLSCertificateFile: /etc/pki/tls/ldapserver.crt
```

```bash
[root@hammer1 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f tls.ldif
```

##### 18. Fix the /etc/openldap/ldap.conf to allow for certs

```bash
[root@hammer1 ~]# vi /etc/openldap/ldap.conf
```

```bash
#
# LDAP Defaults
#

# See ldap.conf(5) for details
# This file should be world readable but not world writable.

#BASE dc=example,dc=com
#URI ldap://ldap.example.com ldap://ldap-master.example.com:666

#SIZELIMIT 12
#TIMELIMIT 15
#DEREF never

# When no CA certificates are specified the Shared System Certificates
# are in use. In order to have these available along with the ones specified # by TLS_CACERTDIR one has to include them explicitly:

TLS_CACERT /etc/pki/tls/ldapserver.crt
TLS_REQCERT never

# System-wide Crypto Policies provide up to date cipher suite which should
# be used unless one needs a finer grinded selection of ciphers. Hence, the
# PROFILE=SYSTEM value represents the default behavior which is in place
# when no explicit setting is used. (see openssl-ciphers(1) for more info)
#TLS_CIPHER_SUITE PROFILE=SYSTEM

# Turning this off breaks GSSAPI used with krb5 when rdns = false
SASL_NOCANON on
```

```bash
[root@hammer1 ~]# systemctl restart slapd
```

#### SSSD Configuration and Realmd join to LDAP

SSSD can connect a server to a trusted LDAP system and authenticate users for access to
local resources. You will likely do this during your career and it is a valuable skill to work with.

##### 1. Install sssd, configure, and validate that the user is seen by the system

```bash
[root@hammer1 ~]# dnf install openldap-clients sssd sssd-ldap oddjob-mkhomedir authselect
[root@hammer1 ~]# authselect select sssd with-mkhomedir --force
[root@hammer1 ~]# systemctl enable --now oddjobd.service
[root@hammer1 ~]# systemctl status oddjobd.service
```

##### 2. Uncomment and fix the lines in /etc/openldap/ldap.conf

```bash
[root@hammer1 ~]# vi /etc/openldap/ldap.conf
```

Output:

`BASE dc=prolug,dc=lan` 
`URI ldap://ldap.ldap.lan/`

##### 3. Edit the sssd.conf file

```bash
[root@hammer1 ~]# vi /etc/sssd/sssd.conf
```

```yaml
[domain/default]
id_provider = ldap
autofs_provider = ldap
auth_provider = ldap
chpass_provider = ldap
ldap_uri = ldap://ldap.prolug.lan/
ldap_search_base = dc=prolug,dc=lan
#ldap_id_use_start_tls = True
#ldap_tls_cacertdir = /etc/openldap/certs
cache_credentials = True
#ldap_tls_reqcert = allow

[sssd]
services = nss, pam, autofs
domains = default

[nss]
homedir_substring = /home
```

```bash
[root@hammer1 ~]# chmod 0600 /etc/sssd/sssd.conf
[root@hammer1 ~]# systemctl start sssd
[root@hammer1 ~]# systemctl status sssd
```

#### 4. Validate that the user can be seen

```bash
[root@hammer1 ~]# id testuser
```

Output:

`uid=15000(testuser) gid=15000 groups=15000`


#### Please reboot the the lab machine when done.

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 3 [Web Book](https://professionallinuxusersgroup.github.io/psc/u3ws.html) ProLUG, 2025.
[^2]: Stigs that involve PAM for RHEL 9 [Webstie](https://docs.rockylinux.org/guides/security/pam/ Source, 2025.
[^3]: configurations of Linux via sssd [Website](https://docs.rockylinux.org/guides/security/authentication/active_directory_authentication) Source, 2025.


