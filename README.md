# OpenVPN on Mikrotik by Gabriel Lami
Configure OpenVPN on Mikrotik professionally(Site-to-Client)

1- First, we choose and create a network for the VPN clients. In this tutorial I will use `192.168.30.0/24`. Than we will create the bridge, IP Pool and DHCP Server.

 - Creation of the bridge where the network addresses will be added. `Bridge->Create new bridge`
![BridgeMikrotikVPN](https://user-images.githubusercontent.com/44748406/192095424-419f230b-a815-4f8e-9ee6-553b7cc6f7d7.png)
 - Creation of the network address. `IP->Addresses`
![NetworkAddess](https://user-images.githubusercontent.com/44748406/192097740-679f4df2-b144-416e-ba6c-08247b2cd722.png)
 - Creation of the IP Pool. You can set a desidered addresses range depending on how many clients will connect to the VPN. `IP->Pool`
![IPPOOL](https://user-images.githubusercontent.com/44748406/192097055-2a06074c-a66a-48a7-9ef4-022a829135a5.png)
 - Creation of the DHCP server. Good to have if you don't want to manually assing VPN ip addresses to the clients. `IP->DHCP Server`
![IPDHCPSERVER](https://user-images.githubusercontent.com/44748406/192097000-25147c4a-a62e-445a-a22b-682e6f744eab.png)
 - Creation of the network. `IP->Networks`
![networks](https://user-images.githubusercontent.com/44748406/192100997-b36564ed-1b6c-4452-a7ba-a83b59b1980d.png)

2- Secondly, we have to create the firewall and NAT rule for the VPN to be able for the clients to communicate with the VPN Server.

- Creation of the firewall filter rule. `IP->Firewall->Filter Rules`
![IPFILTERRULES](https://user-images.githubusercontent.com/44748406/192097762-349de4da-9a68-4963-b814-3b333d9c7eae.png)
- Creation of the NAT rule(only if you don't have this rule already). `IP->Firewall->NAT`
![NATRULE](https://user-images.githubusercontent.com/44748406/192098130-ac7b040a-7afe-4d5c-94d7-da9df854b818.png)

3- Third, we will create and export certificates(CA,server and client) needed for authentication between VPN server and client.

- Creation of CA certificate. `System->Certificates`

  On General Tab:
   -Name: CA
   -Common Name: CA
   -Key Size: 2048
  
  On Key Usage Tab:
   -key cert. sing
   -crl sing
  
  On Sing button click:
   -CA CRL HOST: Your public IP address
  
- Creation of server certificate. `System->Certificates`
  
  On General Tab:
   -Name: server
   -Common Name: server
   -Key Size: 2048
  
  On Key Usage Tab:
   -digital signature
   -key encipherment
   -tls server
  
  On Sing button click:
   -CA: CA

- Creation of client certificate. `System->Certificates`
  
  On General Tab:
   -Name: client
   -Common Name: client
   -Key Size: 2048
  
  On Key Usage Tab:
   -tls client
  
  On Sing button click:
   -CA: CA
  
   ![Certificatescreation](https://user-images.githubusercontent.com/44748406/192099452-94fbe97e-a68a-44ad-b6f2-442fb5ff0f8f.png)

- Export certificates. `System->Certificates`

  Right click on CA and server certificates and click export and than export.
  Right click on client certificate and click export, enter a passphrase and than export.
  
  After export you can find the certificates on `Files` where you can download them because will need them on the client part when you will connect to the VPN.
  ![exportcertificates](https://user-images.githubusercontent.com/44748406/192100024-4636e14b-fa4b-47e8-b5e2-a03470c115dc.png)
  
 4- Forth, we will create the OpenVPN server, profile and secret credentials for the user who will connect to this VPN server.
 
  - Creation of the OpenVPN server. `PPP->Interface->OVPN Server`
![OVPNSERVER](https://user-images.githubusercontent.com/44748406/192100634-bdc14c96-2590-465c-8120-3451949f4be0.png)

  - Creation of the profile which on this case it will be the default-encryption. `PPP->Profiles`
![Profile](https://user-images.githubusercontent.com/44748406/192100861-1827ca05-caa7-4196-8253-9d06d294e578.png)

  - Creation of the user credentials which will be used to connect to the VPN server. `PPP->Secrets`
![secret](https://user-images.githubusercontent.com/44748406/192101298-28a028e6-b370-4722-b022-2daa3c806b08.png)

 5- In this last step we will configure OpenVPN client to be able to connect through VPN
 
  - Convert three certificates from PEM plain text to pmcks encrypted(one file) certificate.

    - Download OpenSSL for Windows on this link and install it.
    https://slproweb.com/products/Win32OpenSSL.html
    
    - Open CMD with admin privileages and cd to the OpenSSL bin directory
      `cd C:\Program Files\OpenSSL-Win64\bin`
    - Copy certificate files into the same directory and run this command
      `openssl pkcs12 -export -in cert_export_client -inkey cert_export_client.key -certfile cert_export_CA.cert -name MyClient -out client.p12`
    - A file named client.p12 will be generated. 

   - We will create the VPN configuration file client.ovpn with the details above
  
     client
     dev tun
     proto tcp-client
     remote 95.107.169.8
     port 1194
     proto tcp
     nobind
     persist-key
     persist-tun
     tls-client
     remote-cert-tls server
     verb 4
     mute 10
     cipher AES-256-CBC
     auth SHA1
     pkcs12 client.p12
     auth-user-pass
     auth-nocache
     #Add here your local network IP if you want only network access only
     route x.x.x.x 255.255.255.0 
     route x.x.x.0 255.255.255.0
     #Use this only if you want to route your VPN traffic
     #redirect-gateway def1
     
     Finally you will have these two files
    ![filess](https://user-images.githubusercontent.com/44748406/192105096-ed4960b0-3273-40ec-a1fa-fd71bd796157.png)
     
   - Install OpenVPN client on you Windows Laptop/PC

     You can download it here
     https://openvpn.net/community-downloads/
     
     Copy those two files created earlier `client.ovpn` and `client.p12` in the VPN config folder like in the photo
     ![filess](https://user-images.githubusercontent.com/44748406/192105096-ed4960b0-3273-40ec-a1fa-fd71bd796157.png)

     Click connect on the client, add the passphrase created earlier when you exported the certificates and after fill 
     the user credentials.
     
     Good job, you should be now connected! If not, check the steps again because you might have done any mistake.
     ![connected](https://user-images.githubusercontent.com/44748406/192104965-dc0d7c5a-e10e-49dd-92c3-867192aec85a.png)
     
     Tahnk you.
