# Instructions

1. Make up a domain name (e.g., www.ilovenetworking.com) and create a certificate signing request (CSR) (e.g., web.csr.pem) for that domain. You will find slides illustrating how to do this at the end of the slide deck for Lecture 3. Write down all the steps and show the CSR you generated. We will provide a file called openssl.conf on Sakai for use with the openssl command-line tool.

2. You need to have the CSR signed by a certificate authority (CA). For this assignment, you will act as the CA. So you'll need to start by creating a certificate for a "root" CA. First create a root key (e.g., ca.key.pem), and then use the root key to create a root certificate (e.g., ca.cert.pem). Write down all the steps and show the certificate you generated.

3. Normally the root certificate authority does not directly sign certificates for domains. Instead, they are signed by intermediate certificate authorities. So first create an intermediate key (e.g., intermediate.key.pem), then use that to create a CSR (e.g., intermediate.csr.pem). Finally, use the root key to sign the CSR, resulting in a signed intermediate certificate (intermediate.cert.pem). Write down all the steps and show the certificate you generated.

4. Next, you will use the intermediate key to sign your web certificate (web.csr.pem). Let's call this signed certificate web.cert.pem. Write down all the steps and show the certificate you generated.