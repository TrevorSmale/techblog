+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 9 - Certificate & Key Madness"
date = "2025-05-31"
description = "Certificate & Key Madness"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

## Intro 👋

In this Unit we look at how Certificates and Keys go beyond Asymmetric encryption with Public / Private. We look at how multiple checks and multiple layers of trust must be used in this mad mad world. [^1]

---

## Worksheet

`Question`

How do these topics align with what you already know about system security?

`Answer`

Well I had felt like I had a clear picture of `Symmetric` and `Asymmetric` encryption modalities. Furthermore, I had a strong prior understanding of `x.509` and `SSH` where Asymmetric encryption is used. Moreover, the procedure of generating Private and subsequent public keys. However, the verbosity and complexity of the required reading has me scratching my head and looking at more sophisticated modality of key generation and exchange eg. `TLS 1.2, 1.3`

`Question`

Were any of the terms or concepts new to you?

`Answer`

`key-transport and/or key-agreement protocols` - a method of establishing a shared secret key between two or more parties 
where one party creates the key and securely delivers it to the others.

`Challenge Values` - dynamic, randomly generated numbers or strings used to initiate authentication.

`nonce` - a unique, random or pseudo-random number used to ensure the security and integrity of data transmitted over a network.
Watch short video about CA and Chain of Trust
Distributed Trust Model
Review the TLS Overview section, pages 4-7 of 
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf
and answer the following questions.
What are the three subprotocols of TLS?
How does TLS apply
Confidentiality
Integrity
Authentication
Anti-replay

`Question`

What are the three subprotocols of TLS?

`Answer`

`The handshake`  used to negotiate the session parameters.

`Change cipher spec` used in TLS 1.0, 1.1, and 1.2 to change the cryptographic parameters of a session.

`Alert protocols`  used to notify the other party of an error condition.

`Question`

How does TLS apply to:
Confidentiality
Integrity
Authentication
Anti-replay

`Answer`

`Confidentiality`

Confidentiality is provided for a communication session by the negotiated encryption algorithm
for the cipher suite and the encryption keys derived from the master secret and random values.

`Integrity`

TLS uses a cipher suite of algorithms and functions, including key establishment, digital signature, confidentiality, and integrity algorithms. In TLS 1.3, the master secret is derived by iteratively invoking an extract-then-expand function with previously derived secrets, used by the negotiated security services to protect the data exchanged between the client and the serve. In TLS 1.3, only AEAD symmetric algorithms are used for confidentiality and integrity.

`Authentication`

Server authentication is performed by the client using the server’s public-key certificate, which
the server presents during the handshake.

`Anti-Replay`

in TLS 1.3 The integrity-protected envelope of the message contains a monotonically increasing sequence number. Once the message integrity is verified, the sequence number of the current message is compared with the sequence number of the previous message.

### Definitions

- `TLS` (Transport Layer Security) A protocol that encrypts data in transit to ensure privacy and integrity.
- `Symmetric Keys` A cryptographic method where the same key is used for both encryption and decryption.
- `Asymmetric Keys` A method using a public/private key pair where one key encrypts and the other decrypts.
- `Non-Repudiation` A guarantee that a sender cannot deny the authenticity of their message or signature.
- `Anti-Replay` A mechanism that prevents attackers from reusing valid data packets to mimic legitimate transactions.
- `Plaintext` Data in a readable and unencrypted format.
- `Cyphertext` Data that has been encrypted and is unreadable without the correct decryption key.
- `Fingerprints` Short unique representations (hashes) of public keys used to verify their authenticity.
- `Passphrase (in key generation)` A user-supplied string that encrypts private keys to protect them from unauthorized access.

### Lab 🧪

`Assignment`

- Complete the lab: https://killercoda.com/het-tanis/course/Linux-Labs/211-setting-up-rsyslog-with-tls

`Answer`

- We generated a 90 day TLS web client certificate. I saved a snippet of the options below.

```bash
Activation/Expiration time.
The certificate will expire in (days): 90
Extensions.
Does the certificate belong to an authority? (y/N): y
Path length constraint (decimal, -1 for no constraint): 
Is this a TLS web client certificate? (y/N): y
Will the certificate be used for IPsec IKE operations? (y/N): y
Is this a TLS web server certificate? (y/N): y
Enter a dnsName of the subject of the certificate: 
Enter a URI of the subject of the certificate: 
Enter the IP address of the subject of the certificate: 
Will the certificate be used for signing (DHE ciphersuites)? (Y/n): y
Will the certificate be used for encryption (RSA ciphersuites)? (Y/n): y
Will the certificate be used for data encryption? (y/N): y
Will the certificate be used to sign OCSP requests? (y/N): y
Will the certificate be used to sign code? (y/N): y
Will the certificate be used for time stamping? (y/N): y
Will the certificate be used for email protection? (y/N): y
Will the certificate be used to sign other certificates? (Y/n): y
Will the certificate be used to sign CRLs? (y/N): y
Will the certificate be used for signing (DHE ciphersuites)? (Y/n): y
Enter the URI of the CRL distribution point: 
X.509 Certificate Information:
Version: 3
Serial Number (hex): 32a1646105dcb6229eba87ad4c08a99a2bb92a99
Validity:
Not Before: Mon Jun 02 03:46:43 UTC 2025
Not After: Sun Aug 31 03:46:48 UTC 2025
Subject: O=prolug
Subject Public Key Algorithm: RSA
Algorithm Security Level: High (3072 bits)
Modulus (bits 3072):
00:e8:c7:f5:6e:7c:23:e3:7e:e7:d0:c5:c4:cf:c0:98
23:5f:1e:f6:5f:5d:87:c6:c8:18:13:cb:5e:1b:1a:88
03:98:4d:55:5d:4d:14:cc:78:8d:83:e3:c5:65:16:8c
41:a8:9f:32:ab:f4:47:3f:84:b2:b8:0d:7c:b3:a6:e7
21:59:13:d2:45:40:60:d6:2c:eb:5a:f3:00:0c:e7:36
06:0f:ca:51:04:92:06:91:80:f0:04:52:d2:66:e3:33
11:7b:8e:f7:e3:22:19:83:c8:dc:c8:f9:18:c7:51:4f
38:6a:d8:07:bf:12:02:f4:5e:0d:52:2e:cc:0b:4e:d9
e0:b2:07:9a:cd:39:99:a7:28:42:e4:67:b0:ff:04:2d
f9:13:8c:0f:19:b5:13:ee:59:a3:e7:e8:f7:a1:e9:92
2e:ce:49:23:3c:0a:b4:29:ca:5d:74:6e:9e:09:ea:fd
72:6a:89:6e:5f:29:d6:0a:44:98:1e:2c:39:66:44:11
4f:47:c5:64:a3:0c:84:2b:fd:32:2e:a9:ce:e7:be:b4
7c:3b:e6:b9:23:98:82:ac:86:20:07:4e:59:84:4d:0c
02:38:76:87:ef:f8:17:05:5b:93:79:25:73:fc:18:f5
4e:1d:ff:84:45:10:7d:46:51:69:ae:73:6d:e9:1e:fd
ff:55:5a:78:4d:f6:cd:44:af:22:0f:b0:18:fb:82:b9
f6:aa:3d:2a:08:00:62:d1:9b:28:50:94:39:98:f5:de
f9:cf:3f:d8:ae:72:68:69:f1:46:97:8f:d5:a6:9a:3e
4c:57:37:5f:69:0e:2f:4e:b6:6e:65:a5:2c:f0:5b:c6
c2:ff:43:b7:4e:b7:56:3f:2b:d8:5d:b9:73:15:ca:81
f1:c3:78:2f:8d:4f:fd:e8:2d:6f:2f:2d:f6:b9:e1:a0
11:f2:56:18:02:5b:8e:07:da:19:43:c1:70:bc:7b:8b
82:2b:02:e2:71:6e:30:9b:18:8d:ed:1f:29:59:86:9d
81
Exponent (bits 24):
01:00:01
Extensions:
Basic Constraints (critical):
Certificate Authority (CA): TRUE
Key Purpose (not critical):
TLS WWW Client.
TLS WWW Server.
Ipsec IKE.
OCSP signing.
Code signing.
Time stamping.
Email protection.
Key Usage (critical):
Digital signature.
Key encipherment.
Data encipherment.
Certificate signing.
CRL signing.
Subject Key Identifier (not critical):
213b20bf44b3446fb14f6cf72b8c2c03a09e292e
Other Information:
Public Key ID:
sha1:213b20bf44b3446fb14f6cf72b8c2c03a09e292e
sha256:7f76aada143491a8ba0721509a3e49f9e72321ed880f7ee64b8e01172989b3d2
Public Key PIN:
pin-sha256:f3aq2hQ0kai6ByFQmj5J+ecjIe2ID37mS44BFymJs9I=
```

`Reading`

- Review Solving the Bottom Turtle[^5]

`Question`

- Does the diagram on page 44 make sense to you for what you did with a certificate authority in this lab?

`Answer`

- Yes it does, we had only setup a portion of this chain of trust, yet it got the idea across of whom we are referring to and how we build a certificate from that referral.

`Assignment`

- Complete the lab: https://killercoda.com/het-tanis/course/Linux-Labs/212-public-private-keys-with-ssh

`Question`

- What is the significance of the permission settings that you saw on the generated public and private key pairs?

`Answer`

- Only the owner has Read/Write permission to the Private key, whereas the public key -- meant to be shared, is Readable by Group and Others.

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 8 [Web Book](https://professionallinuxusersgroup.github.io/psc/u9intro.html) ProLUG, 2025.
[^2]: Config MGMT [Wikipedia](https://en.wikipedia.org/wiki/Configuration_management) Wikipedia, 2025.
[^3]: Building Secure and Reliable Systems [Web Book](https://google.github.io/building-secure-and-reliable-systems/raw/ch14.html#treat_configuration_as_code) Google, 2025.
[^4]: Telemetry [Web Book](https://grafana.com/docs/tempo/latest/introduction/telemetry/) Grafana Labs, 2025.
[^5]: Solving the Bottom Turtle [Web Book](https://spiffe.io/pdf/Solving-the-bottom-turtle-SPIFFE-SPIRE-Book.pdf) Spiffe.io,2020. 

