# Instructions

1. Make up a domain name (e.g., www.ilovenetworking.com) and create a certificate signing request (CSR) (e.g., web.csr.pem) for that domain. You will find slides illustrating how to do this at the end of the slide deck for Lecture 3. Write down all the steps and show the CSR you generated. We will provide a file called openssl.conf on Sakai for use with the openssl command-line tool.

    - Follow all instructions below before coming back to this one
    - Enter the following into Git Bash: `openssl req -noout -text -in intermediate/csr/web.csr.pem`

2. You need to have the CSR signed by a certificate authority (CA). For this assignment, you will act as the CA. So you'll need to start by creating a certificate for a "root" CA. First create a root key (e.g., ca.key.pem), and then use the root key to create a root certificate (e.g., ca.cert.pem). Write down all the steps and show the certificate you generated.

    - Enter the following into Git Bash
        - `mkdir certs intermediate private`
        - `mkdir intermediate/private intermediate/certs`
        - `chmod 700 private/`
        - `chmod 700 intermediate/private/`
        - `touch index.txt`
        - `echo 1000 > serial`
        - `openssl genrsa -aes256 -out private/ca.key.pem 4096`
    - Enter and verify a passphrase for private/ca.key.pem
    - Enter the following into Git Bash: `openssl req -config openssl.conf -key  private/ca.key.pem -new -x509 -sha256 -extensions v3_ca -out certs/ca.cert.pem`
    - Enter passphrase for private/ca.key.pem
    - Enter information that will be incorporated into the certificate request
    - Enter the following into Ubuntu WSL prompt: `openssl x509 -noout -text -in certs/ca.cert.pem | more`

3. Normally the root certificate authority does not directly sign certificates for domains. Instead, they are signed by intermediate certificate authorities. So first create an intermediate key (e.g., intermediate.key.pem), then use that to create a CSR (e.g., intermediate.csr.pem). Finally, use the root key to sign the CSR, resulting in a signed intermediate certificate (intermediate.cert.pem). Write down all the steps and show the certificate you generated.

    - Enter the following into Git Bash: `openssl genrsa -aes256 -out intermediate/private/intermediate.key.pem 4096`
    - Enter and verify a passphrase for intermediate/private/intermediate.key.pem
    - Enter the following into Git Bash
        - `mkdir intermediate/csr`
        - `openssl req -config openssl.conf -new -sha256 -key intermediate/private/intermediate.key.pem -out intermediate/csr/intermediate.csr.pem`
    - Enter passphrase for intermediate/private/intermediate.key.pem
    - Enter information that will be incorporated into the certificate request
    - Enter the following into Git Bash
        - `mkdir newcerts`
        - `openssl ca -config openssl.conf -extensions v3_intermediate_ca -days 3650 -notext -md sha256 -keyfile private/ca.key.pem -cert certs/ca.cert.pem -in intermediate/csr/intermediate.csr.pem -out intermediate/certs/intermediate.cert.pem`
    - Enter passphrase for private/ca.key.pem
    - Sign the certificate (enter "y" when prompted) and commit (enter "y" when prompted)
    - Enter the following into Git Bash
        - `cat index.txt`
        - `cat serial`
    - Enter the following into Ubuntu WSL Prompt: `openssl x509 -noout -text -in intermediate/certs/intermediate.cert.pem | more`

4. Next, you will use the intermediate key to sign your web certificate (web.csr.pem). Let's call this signed certificate web.cert.pem. Write down all the steps and show the certificate you generated.

    - Enter the following into Git Bash
        - `openssl verify -CAfile certs/ca.cert.pem intermediate/certs/intermediate.cert.pem`
        - `openssl genrsa -aes256 -out intermediate/private/web.key.pem 2048`
    - Enter and verify a passphrase for intermediate/private/web.key.pem
    - Enter the following into Git Bash
        - `chmod 400 intermediate/private/web.key.pem`
        - `openssl req -config openssl.conf -key intermediate/private/web.key.pem -new -sha256 -out intermediate/csr/web.csr.pem`
    - Enter passphrase for intermediate/private/web.key.pem
    - Enter information that will be incorporated into the certificate request
    - Enter the following into Git Bash: `openssl ca -config openssl.conf -keyfile intermediate/private/intermediate.key.pem -cert intermediate/certs/intermediate.cert.pem -extensions server_cert -days 375 -notext -md sha256 -in intermediate/csr/web.csr.pem -out intermediate/certs/web.cert.pem`
    - Enter passphrase for intermediate/private/intermediate.key.pem
    - Sign the certificate (enter "y" when prompted) and commit (enter "y" when prompted)
    - Enter the following into Ubuntu WSL Prompt: `openssl x509 -noout -text -in intermediate/certs/web.cert.pem | more`
