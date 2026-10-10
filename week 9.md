# Week 9 – Digital Certificates and Secure Web Server

## Overview

In Week 9, I worked with digital certificates, Certificate Authorities (CAs), HTTPS, and Apache web-server security.

The tutorial included two main tasks:

- analysing trusted root certificates and website certificates
- deploying a secure HTTPS web server using a custom Certificate Authority

The activities helped me understand how certificate trust works, how a CA signs certificates, and how HTTPS is configured and verified.

---

# Task 1 – Analyse Digital Certificates

## Root Certificate Analysis

I opened the browser certificate manager and examined the trusted root certificate **Amazon Root CA 1**.

The certificate showed:

- Common Name: Amazon Root CA 1
- Organisation: Amazon
- Issued To: Amazon Root CA 1
- Issued By: Amazon Root CA 1
- validity period
- SHA-256 certificate fingerprint
- SHA-256 public-key fingerprint

Because the **Issued To** and **Issued By** fields are the same, this is a self-signed root certificate.

![Amazon Root CA 1](images/week9-task1-root-certificate.png)

*Figure 1: Amazon Root CA 1 trusted root certificate details.*

---

## Website Certificate Analysis

I inspected the HTTPS certificate for:

`www.cqu.edu.au`

The certificate showed:

- Common Name: `www.cqu.edu.au`
- Issuer: `WE1`
- Issuer Organisation: Google Trust Services
- validity period
- SHA-256 certificate fingerprint
- SHA-256 public-key fingerprint

Unlike the root certificate, this website certificate is not self-signed because the **Issued To** and **Issued By** fields are different.

This demonstrates how an HTTPS website certificate is issued by a Certificate Authority and validated through a chain of trust.

![CQU Website Certificate](images/week9-task1-website-certificate.png)

*Figure 2: HTTPS certificate details for www.cqu.edu.au.*

---

## Discussion – Trust in Certificate Authorities

Root certificates are trusted because they are stored in the trusted certificate stores maintained by browsers and operating systems.

However, trust in a Certificate Authority is not absolute. If a trusted CA is compromised or incorrectly issues a certificate, an attacker may obtain a certificate that clients accept as valid.

For this reason, browsers and operating systems maintain trusted root stores and may remove or distrust CAs that fail to meet required security standards.

Therefore, I would not assume that every Certificate Authority is equally trustworthy. Trust depends on the CA's security practices, auditing, operational controls, and reputation.

---

# Task 2 – Deploy a Secure Web Server

## Objective

The objective of this task was to configure an Apache web server on node3 to use HTTPS with a custom Certificate Authority.

The virtnet topology used was:

- node1 – client
- node2 – router
- node3 – Apache web server

The major steps included:

- testing Apache over HTTP
- creating a Certificate Authority
- configuring the CA directory structure
- configuring the CA policy
- creating a server private key
- creating a Certificate Signing Request
- signing the server certificate
- verifying the certificate
- enabling Apache SSL
- testing HTTPS from node1
- verifying the certificate chain from the client

---

## Step 1 – Verify Apache Web Server

On node3, I checked that Apache was running.

Command used:

`sudo systemctl status apache2`

The output showed:

`Active: active (running)`

![Apache Running](images/week9-task2-apache-running.png)

*Figure 1: Apache web server running successfully on node3.*

---

## Step 2 – Test HTTP Before HTTPS

Before enabling HTTPS, I tested normal HTTP connectivity from node1.

The Apache default page successfully loaded using Lynx.

![HTTP Before HTTPS](images/week9-task2-http-before-https.png)

*Figure 2: Apache default page accessed from node1 before HTTPS configuration.*

---

## Step 3 – Create the Certificate Authority

On node3, I created an RSA private key for the Certificate Authority.

I then created a self-signed CA certificate.

The CA certificate included information such as:

- Country: AU
- State: QLD
- Locality: Brisbane
- Organisation: CQU
- Organisational Unit: Authority
- Common Name: MyUni CA

I verified that both files were successfully created:

- `cakey.pem`
- `cacert.pem`

![CA Created](images/week9-task2-ca-created.png)

*Figure 3: CA private key and CA certificate successfully created.*

---

## Step 4 – Create the CA Directory Structure

I created the required CA directory structure:

- `demoCA/certs`
- `demoCA/crl`
- `demoCA/newcerts`
- `demoCA/private`
- `demoCA/index.txt`
- `demoCA/serial`

The CA certificate was stored in the main `demoCA` directory and the CA private key was stored in the private directory.

![CA Structure](images/week9-task2-demoCA-structure.png)

*Figure 4: Certificate Authority directory structure.*

---

## Step 5 – Configure the CA Policy

I edited the OpenSSL configuration file and configured the CA policy.

The important policy settings included:

`countryName = match`

`stateOrProvinceName = optional`

`organizationName = optional`

`organizationalUnitName = optional`

`commonName = supplied`

`emailAddress = optional`

![CA Policy](images/week9-task2-ca-policy.png)

*Figure 5: OpenSSL CA policy configuration.*

---

## Step 6 – Create the Web Server Key and CSR

I generated a new RSA private key for the web server and then created a Certificate Signing Request.

The server hostname used was:

`www.myuni.edu`

The generated files were:

- `privkey-www.myuni.edu.pem`
- `certreq-www.myuni.edu.csr`

![Web Key and CSR](images/week9-task2-web-key-csr.png)

*Figure 6: Web-server private key and Certificate Signing Request successfully created.*

---

## Step 7 – Sign and Verify the Server Certificate

The CSR was signed using the custom Certificate Authority.

The server certificate was created as:

`cert-www.myuni.edu.pem`

I then verified the certificate against the CA using OpenSSL.

The result was:

`cert-www.myuni.edu.pem: OK`

![Certificate Signed and Verified](images/week9-task2-certificate-signed-verified.png)

*Figure 7: Web-server certificate signed by the CA and successfully verified.*

---

## Step 8 – Enable HTTPS in Apache

I configured Apache to use the new HTTPS certificate and private key.

The SSL configuration used:

`ServerName www.myuni.edu`

`SSLCertificateFile /etc/ssl/certs/cert-www.myuni.edu.pem`

`SSLCertificateKeyFile /etc/ssl/private/privkey-www.myuni.edu.pem`

`SSLCACertificateFile /etc/ssl/certs/cert-ourca.crt`

I then enabled SSL and the default SSL site.

Apache was restarted successfully and showed:

`Active: active (running)`

![Apache HTTPS Enabled](images/week9-task2-apache-https-enabled.png)

*Figure 8: Apache running successfully after HTTPS configuration.*

---

## Step 9 – Initial HTTPS Test

From node1, I tested:

`https://www.myuni.edu`

The HTTPS connection reached the web server, but Lynx initially displayed:

`The certificate is NOT trusted`

This occurred before the custom CA trust configuration was fully recognised by Lynx.

![HTTPS Before CA Trust](images/week9-task2-https-before-ca-trust.png)

*Figure 9: Initial HTTPS connection showing certificate trust warning.*

---

## Step 10 – Verify the Certificate Chain

The client trust store was updated with the custom CA certificate.

I then used OpenSSL from node1 to verify the TLS certificate chain presented by the server.

The important result was:

`Verify return code: 0 (ok)`

This confirmed that:

- the server certificate was valid
- the certificate was correctly signed by the custom CA
- node1 trusted the CA
- the TLS certificate chain was successfully verified

![OpenSSL Verification](images/week9-task2-openssl-verify-ok.png)

*Figure 10: OpenSSL verification returning code 0, confirming successful certificate validation.*

---

## Step 11 – Access the HTTPS Web Page

After certificate verification, I accessed the Apache web server over HTTPS using Lynx.

The Apache default page loaded successfully from node1.

![HTTPS Page Loaded](images/week9-task2-https-page-loaded.png)

*Figure 11: Apache web page successfully loaded over HTTPS.*

An additional screenshot of the loaded page was also recorded.

![HTTPS Page Loaded Additional View](images/week9-task2-https-page-loaded1.png)

*Figure 12: Additional view of the HTTPS Apache page on node1.*

---

## Task 2 Result

The secure web-server deployment was completed successfully.

The final configuration demonstrated that:

- Apache was running on node3
- a custom Certificate Authority was created
- a web-server CSR was generated
- the CA signed the server certificate
- the certificate was verified successfully
- HTTPS was enabled in Apache
- the client connected to the server over HTTPS
- OpenSSL returned `Verify return code: 0 (ok)`
- the Apache web page was accessible through HTTPS

---

# Week 9 Outcome

In Week 9, I learned how Public Key Infrastructure and HTTPS work in practice.

The activities demonstrated the relationship between:

- root Certificate Authorities
- server certificates
- private keys
- Certificate Signing Requests
- certificate chains
- trusted CA stores
- Apache SSL configuration
- HTTPS client verification

The most important practical outcome was successfully creating a custom CA, issuing a certificate for `www.myuni.edu`, configuring Apache to use HTTPS, and verifying the certificate chain from node1 with OpenSSL.
