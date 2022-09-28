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
                            
                            
   ![Picture2](https://user-images.githubusercontent.com/90408697/192580040-dfd3e247-2ee8-45ac-88ac-4347343f3ad5.png)
                            
                            
                                                            Countru Name:ET
                                                            State or province: AA
                                                            Locality Name:AA
                                                            Organization name:AAU
                                                            Organization Unit name:SW
                                                            Commom Name:HenokCA.com
                                                            Email_Address:henokmekuanint79@gmail.com
                                                            Password:******
                                                            
![Picture3](https://user-images.githubusercontent.com/90408697/192578760-16867e23-2604-4600-821a-c33a722ed4fc.png)
                              
                              
                              C.To view the certificate we will use the command

                                    Opessl x509 -in ca.crt -text -noout
                                    
                                    
  ![Picture4](https://user-images.githubusercontent.com/90408697/192576276-7ea3ac85-cb01-46b9-be95-022f1a0a019c.png)
                                    
                                    
 ![Picture5](https://user-images.githubusercontent.com/90408697/192591431-b065b79f-196a-43be-98e3-39e0e26190e9.png)
                                    
                              2.2 Task 2: Creating a Certificate for SEEDPKILab2020.com

                              Now, we become a root CA, we are ready to sign digital certificates for our customers. Our first customer is a company called SEEDPKILab2020.com. For this company to get a digital certificate from a CA, it needs to go through three steps. 
                              
                              Step 1: Generate public/private key pair
                              
                          The company needs to first create its own public/private key pair. We can run the following command to generate an RSA key pair (both private and public keys). You will also be required to provide a password to encrypt the private key (using the AES-128 encryption algorithm, as is specified in the command option). The keys will be stored in the file server.key:
                          $ openssl genrsa -aes128 -out server.key 1024

                          The server.key is an encoded text file (also encrypted), so you will not be able to see the actual content, such as the modulus, private exponents, etc. To see those, you can run the following command:
                           $ openssl rsa -in server.key -text
                           
 ![Picture6](https://user-images.githubusercontent.com/90408697/192577668-71676e82-c1b9-48b9-bd94-f015097c5d88.png)
                            
                                                2.View content of server.key 
                                                
                                              a.openssl rsa -in server.key –text
                                              
 ![Picture7](https://user-images.githubusercontent.com/90408697/192576301-ae46bf96-5179-4980-a269-ae18d5a31f7d.png)
                        
 ![Picture8](https://user-images.githubusercontent.com/90408697/192576396-ff010e8a-f7f6-4842-bec4-20a3b2ff956e.png)
                        
![Picture9](https://user-images.githubusercontent.com/90408697/192576413-c7ba9eaa-d036-41f8-bd65-074c8263a786.png)


                                          Step 2: Generate a Certificate Signing Request (CSR).
                                          
                                          
                 Once the company has the key file, it should generates a Certificate Signing Request (CSR), which basically includes the company’s public key. The CSR will be sent to the CA, who will generate a certificate for the key (usually after ensuring that identity information in the CSR matches with the server’s true identity). Please use SEEDPKILab2020.com as the common name of the certificate request

                                    $ openssl req -new -key server.key -out server.csr -config openssl.cnf
                                    
 ![Picture10](https://user-images.githubusercontent.com/90408697/192592604-d46ba0c9-b43b-41b9-a69b-6d1f6da0b830.png)

![Picture11](https://user-images.githubusercontent.com/90408697/192576443-e834cf18-232f-46df-9cf6-7844ef09c79a.png)
                                  
                                  
                                  Step 3: Generating Certificates.
                                  
                                  
                                  The CSR file needs to have the CA’s signature to form a certificate. In the real world, the CSR files are usually sent to a trusted CA for their signature. In this lab, we will use our own trusted CA to generate certificates. The following command turns the certificate signing request (server.csr) into an X509 certificate (server.crt), using the CA’s ca.crt and ca.key:
                                  
                                   $ openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key \ -config openssl.cnf
                                   
                                  If OpenSSL refuses to generate certificates, it is very likely that the names in your requests do not match with those of CA. The matching rules are specified in the configuration file (look at the [policy match] section). You can change the names of your requests to comply with the policy, or you can change the policy. The configuration file also includes another policy (called policy anything), which is less restrictive.
                                  
                                  You can choose that policy by changing the following line:
                                   "policy = policy_match" change to "policy = policy_anything".
                                   
                                  3.Generating Certificates 
                                  
                                  a.openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config openssl.cnf
                                  
![Picture12](https://user-images.githubusercontent.com/90408697/192594637-54202e2a-f805-4b66-9f06-3ba1f8c164bd.png)
![Picture13](https://user-images.githubusercontent.com/90408697/192576479-cb7c84da-fae2-447f-be39-925821269766.png)
                             
                             4.Set the policy restriction to be less restrictive 
                             
                                a.In the policy section, set policy = policy_anything
                                
                                
![Picture36](https://user-images.githubusercontent.com/90408697/192601624-bbf0bb44-bf11-4850-959e-e7bf798b6fa9.png)

                                
                                          2.3 Task 3: Deploying Certificate in an HTTPS Web Server

                                           In this lab, we will explore how public-key certificates are used by websites to secure web browsing. We will set up an HTTPS website using openssl’s built-in web server.

                                          Step 1: Configuring DNS. We choose SEEDPKILab2020.com as the name of our website. To get our computers recognize this name, let us add the following entry to /etc/hosts; this entry basically maps the hostname SEEDPKILab2020.com to our localhost.

![Picture14](https://user-images.githubusercontent.com/90408697/192576540-3f698ef6-09da-4fb5-8b67-53bb2a1ed763.png)

                                          Step 2: Configuring the web server. Let us launch a simple web server with the certificate generated in the previous task. OpenSSL allows us to start a simple web server using the s server command:
                                          
   ![Picture15](https://user-images.githubusercontent.com/90408697/192576577-3b4fd984-d7c2-431a-8004-223e4cd790b9.png)
                                          
                                           Step 3: The third step is importing  our certeficate in the firefox browser
                                                    
                                                    1.Open firefox
                                                    2.Go to edit tab
                                                    3.Preference
                                                    4.Privacy and Security
                                                    5.View Certificate
                                                    6.Import Certeficate
                                                    7.Load ca.crt

  ![Picture16](https://user-images.githubusercontent.com/90408697/192576600-71ee0c8a-611e-4fc5-8afc-662f389c8c4e.png)
                         Step 4:
                         
 ![Picture17](https://user-images.githubusercontent.com/90408697/192576631-6ab6773e-64db-4b64-9948-63fdc7067c9a.png)

 ![Picture18](https://user-images.githubusercontent.com/90408697/192578055-794275c5-03ee-4a9a-819b-546a7536046d.png)
                          
                            A.There will not be any change after changing the server. The information that was displayed previously didn’t change.
                            
                            B.Since there is a common name in the certeficate accessing https://localhost:4433 result certeficate invalidation.
                            
                            2.4 Task 4: Deploying Certificate in an Apache-Based HTTPS Website
                            
                             The HTTPS server setup using openssl’s s server command is primarily for debugging and demonstration purposes. In this lab, we set up a real HTTPS web server based on Apache. The Apache server, which is already installed in our VM, supports the HTTPS protocol. To create an HTTPS website, we just need to configure the Apache server, so it knows where to get the private key and certificates. We give an example in the following to show how to enable HTTPS for a website www.example.com. You task is to do the same for SEEDPKILab2020.com using the certificate generated from previous tasks. An Apache server can simultaneously host multiple websites. It needs to know the directory where a website’s files are stored. 

                            Step 1:  The forst Step is setting up the  HTTP website
                            
 ![Picture19](https://user-images.githubusercontent.com/90408697/192603141-2f416e79-ddfa-4ada-b7de-16e33562151d.png)

 ![Picture20](https://user-images.githubusercontent.com/90408697/192578105-4339e3db-fcfb-4259-825a-b76b56d5910c.png)
                            
                            Step2. SEED PKI Lab 2021.com  to the Virtual Host Entry.
                            
 ![Picture21](https://user-images.githubusercontent.com/90408697/192578129-fcb22d19-444a-4920-b26a-d9855e75bb10.png)

                            Step3: Adding VirtualHost to default-ssl
                            
 ![Picture22](https://user-images.githubusercontent.com/90408697/192604121-cc6685a8-a29e-4db3-886d-2c11dfc360a1.png)
                            
                            Step4: SSL module is enabled and apache is restarted
                            
 ![Picture23](https://user-images.githubusercontent.com/90408697/192578177-d07b73d2-4c90-4437-86f8-5b7fb6341b91.png)
                            
 ![Picture24](https://user-images.githubusercontent.com/90408697/192578189-b27523e0-7b27-48c6-9617-ec92ff4c9df1.png)
 
                          Step5: Lanching SEEDPKILab2021.com
                          
 ![Picture25](https://user-images.githubusercontent.com/90408697/192578200-2e1feb75-2f23-416f-995f-ed0f7db7006a.png)


                                                    2.5 Task 5: Launching a Man In The Middle

                                                    Attack In this task, we will show how PKI can defeat Man-In-The-Middle (MITM) attacks. Figure 1 depicts how MITM attacks work. Assume Alice wants to visit example.com via the HTTPS protocol. She needs to get the public key from the example.com server; Alice will generate a secret, and encrypt the secret using the server’s public key, and send it to the server. If an attacker can intercept the communication between Alice and the server, the attacker can replace the server’s public key with its own public key. Therefore, Alice’s secret is actually encrypted with the attacker’s public key, so the attacker will be able to read the secret. The attacker can forward the secret to the server using the server’s public key. The secret is used to encrypt the communication between Alice and server, so the attacker can decrypt the encrypted communication.
                                                    
                                                    
                                                    Step 1: Setting up the malicious website. 
                                                    
                                                    
                                                    In Task 4, we have already set up an HTTPS website for SEEDPKILab2020.com. We will use the same Apache server to impersonate example.com (or the site chosen by students). To achieve that, we will follow the instruction in Task 4 to add a VirtualHost entry to Apache’s SSL configuration file: the ServerName should be example.com, but the rest of the configuration can be the same as that used in Task 4. Our goal is the following: when a user tries to visit example.com, we are going to get the user to land in our server, which hosts a fake website for example.com. If this were a social network website, The fake site can display a login page similar to the one in the target website. If users cannot tell the difference, they may type their account credentials in the fake webpage, essentially disclosing the credentials to the attacker.
Step 2: Becoming the man in the middle There are several ways to get the user’s HTTPS request to land in our web server. One way is to attack the routing, so the user’s HTTPS request is routed to our web server. Another way is to attack DNS, so when the victim’s machine tries to find out the IP address of the target web server, it gets the IP address of our web server. In this task, we use “attack” DNS. Instead of launching an actual DNS cache poisoning attack, we simply modify the victim’s machine’s /etc/hosts file to emulate the result of a DNS cache positing attack (the IP Address in the following should be replaced by the actual IP address of the malicious server).

                                            Step1:SEED PKI Lab 2021.com  to the Virtual Host Entry.
                                            
                                            
 ![Picture26](https://user-images.githubusercontent.com/90408697/192578244-55d2a82b-e654-4ea5-be31-e0a7f151d5f3.png)
 
                                               Step2: Restarting the apahce 2
                                               
  ![Picture27](https://user-images.githubusercontent.com/90408697/192578260-3c3e68ef-74b3-46d8-ac02-9bd413f7000b.png)


                                                    Step 3: Browse the target website. With everything set up, now visit the target real website, and see what your browser would say. Please explain what you have observed.

![Picture28](https://user-images.githubusercontent.com/90408697/192578301-d943f97c-17dc-4d4d-950c-f2834df52a89.png)


                                        2.6 Task 6: Launching a Man In The Middle Attack with a Compromised CA

The first thing to launch a man in the Middle attack with a Compromised CA is to have a URI that redirects to other page. We have seen that we can create any Certificate with any domain page. So we will try to Create a Certeficaete using youtube.com

![Picture29](https://user-images.githubusercontent.com/90408697/192578318-3d299d7c-1af3-4823-a586-09acb0efa7b6.png)

                                Step1:Generating a certeficate using the same Doman name with youtube.com
                                
![Picture30](https://user-images.githubusercontent.com/90408697/192578325-a5d7229d-e681-4366-9096-7d84c00de281.png)
![Picture31](https://user-images.githubusercontent.com/90408697/192578342-c953ab42-4309-4b75-a52d-fd45c53a54b3.png)

                                Step2: Configuring the Server and adding the IP address and the domain name of the desired arttack
                                
![Picture32](https://user-images.githubusercontent.com/90408697/192578356-2a802cf2-4ee1-4d51-8433-05910b60e515.png)
![Picture33](https://user-images.githubusercontent.com/90408697/192578409-55f97e17-8435-4191-8a82-ca601b9ac764.png)

                                 Step3: Adding VirtualHost with domain name youtube.com and Document Root /var/www/seed
                                 
![Picture34](https://user-images.githubusercontent.com/90408697/192578425-f25103e3-54a8-4dd6-8f81-26e443e1db5f.png)

                                  Step4: Browsing to https://www.youtube.com
![Picture35](https://user-images.githubusercontent.com/90408697/192578446-84ede54d-7efd-4d5c-8294-7adaf8a65b25.png)
