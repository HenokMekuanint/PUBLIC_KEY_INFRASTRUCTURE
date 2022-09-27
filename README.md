# PUBLIC_KEY_INFRASTRUCTURE

Table of Contents 

Abstract 
Task 1: Becoming a Certificate Authority (CA) 

Task 2: Creating a Certificate for SEEDPKILab2021.com 

Task 3: Deploying Certificate in an HTTPS Web Server 

Task 4: Deploying Certificate in an Apache-Based HTTPS Website

Task 5: Launching a Man-In-The-Middle Attack 

Task 6: Launching a Man-In-The-Middle Attack with a Compromised CA 

Conclusion




Absrtact
Public Key infrastructure (PKI) Focuses on the protection os sensetive data, it provides  a unique digital identities for users . PKI aims the presence of secures end to end communication.

Infrastructure (PKI) Lab

Overview Public key cryptography is the foundation of today’s secure communication, but it is subject to man-in-themiddle attacks when one side of communication sends its public key to the other side. The fundamental problem is that there is no easy way to verify the ownership of a public key, i.e., given a public key and its claimed owner information, how do we ensure that the public key is indeed owned by the claimed owner? The Public Key Infrastructure (PKI) is a practical solution to this problem. The learning objective of this lab is for students to gain the first-hand experience on PKI. SEED labs have a series of labs focusing on the public-key cryptography, and this one focuses on PKI. By doing the tasks in this lab, students should be able to gain a better understanding of how PKI works, how PKI is used to protect the Web, and how Man-in-the-middle attacks can be defeated by PKI. Moreover, students will be able to understand the root of the trust in the public-key infrastructure, and what problems will arise if the root trust is broken. This lab covers the following topics:
• Public-key encryption
• Public-Key Infrastructure (PKI)
• Certificate Authority (CA) and root CA 
• X.509 certificate and self-signed certificate
• Apache, HTTP, and HTTPS 
• Man in the middle attacks


2 Lab Tasks 

2.1 Task 1: Becoming a Certificate Authority (CA) 

A Certificate Authority (CA) is a trusted entity that issues digital certificates. The digital certificate certifies the ownership of a public key by the named subject of the certificate. A number of commercial CAs are treated as root CAs; VeriSign is the largest CA at the time of writing. Users who want to get digital certificates issued by the commercial CAs need to pay those CAs. In this lab, we need to create digital certificates, but we are not going to pay any commercial CA. We will become a root CA ourselves, and then use this CA to issue certificate for others (e.g. servers). In this task, we will make ourselves a root CA, and generate a certificate for this CA. Unlike other certificates, which are usually signed by another CA, the root CA’s certificates are self-signed. Root CA’s certificates are usually pre-loaded into most operating systems, web browsers, and other software that rely on PKI. Root CA’s certificates are unconditionally trusted. The Configuration File openssl.conf. In order to use OpenSSL to create certificates, you have to have a configuration file. The configuration file usually has an extension .cnf. It is used by three OpenSSL commands: ca, req and x509. The manual page of openssl.conf can be found using Google search. You can also get a copy of the configuration file from /usr/lib/ssl/openssl.cnf. Certificate Authority (CA). As we described before, we need to generate a self-signed certificate for our CA. This means that this CA is totally trusted, and its certificate will serve as the root certificate. You can run the following command to generate the self-signed certificate for the CA: $ openssl req -new -x509 -keyout ca.key -out ca.crt -config openssl.cnf You will be prompted for information and a password. Do not lose this password, because you will have to type the passphrase each time you want to use this CA to sign certificates for others. You will also be asked to fill in some information, such as the Country 

Name, Common Name, etc. The output of the command are stored in two files: ca.key and ca.crt. The file ca.key contains the CA’s private key, while ca.crt contains the public-key certificate.
                                          Setup
                                          Cp/usr/lib/ssl/openssl.cnf ./
                                          Mkdir demoCA && cd demoCA
                                          Mkdir certs crl newcerts
                                          Touch index.txt serial
                                          Echo 00>serial
                                          Cd ..
          ![Picture1](https://user-images.githubusercontent.com/90408697/192576223-c4fb5649-c231-47f2-95ce-3519e1f7154b.png)
          
          
          Generate a single certeficate

                            To generate a certeficate there are some information must be filled. Those information are filled in the above command.
                            To request a new certeficate we will use the command
                            Openssl req -new -x509 -keyout ca.keys -out ca.crt -config openssl.cnf
                            
                            
                            ![Picture2](https://user-images.githubusercontent.com/90408697/192576254-23f5508b

                                          
