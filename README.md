# OpenVPN on Mikrotik by Gabriel Lami
Configure OpenVPN on Mikrotik(Site-to-Client)

1- First, we choose and create a network for the VPN clients. In this tutorial I will use `192.168.30.0/24`. Than we will create the bridge, IP Pool and DHCP Server.

 - Creation of the bridge where the network addresses will be added. `Bridge->Create new bridge`
![BridgeMikrotikVPN](https://user-images.githubusercontent.com/44748406/192095424-419f230b-a815-4f8e-9ee6-553b7cc6f7d7.png)
 - Creation of the network address. `IP->Addresses`
![NetworkAddess](https://user-images.githubusercontent.com/44748406/192097740-679f4df2-b144-416e-ba6c-08247b2cd722.png)
 - Creation of the IP Pool. You can set a desidered addresses range depending on how many clients will connect to the VPN. `IP->Pool`
![IPPOOL](https://user-images.githubusercontent.com/44748406/192097055-2a06074c-a66a-48a7-9ef4-022a829135a5.png)
 - Creation of the DHCP server. Good to have if you don't want to manually assing VPN ip addresses to the clients. `IP->DHCP Server`
![IPDHCPSERVER](https://user-images.githubusercontent.com/44748406/192097000-25147c4a-a62e-445a-a22b-682e6f744eab.png)

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
  
  
  
