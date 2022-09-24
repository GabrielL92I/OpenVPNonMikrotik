# OpenVPN on Mikrotik by Gabriel Lami
Configure OpenVPN on Mikrotik(Site-to-Client)

1- First we choose and create a network for the VPN clients. In this tutorial I will use `192.168.30.0/24`.

 - Creation of the bridge where the network addresses will be added. 'Bridge->Create new bridge'
![BridgeMikrotikVPN](https://user-images.githubusercontent.com/44748406/192095424-419f230b-a815-4f8e-9ee6-553b7cc6f7d7.png)
 - Creation of the network address. 'IP->Addresses'
 ![NetworkAddess](https://user-images.githubusercontent.com/44748406/192096389-b3d34e94-10e3-47ac-b85c-0b842a6c9aee.png)
 - Creation of the IP Pool. You can set a desidered addresses range depending on how many clients will connect to the VPN. 'IP->Pool'
![IPPOOL](https://user-images.githubusercontent.com/44748406/192097055-2a06074c-a66a-48a7-9ef4-022a829135a5.png)
 - Creation of the DHCP server. Good to have if you don't want to manually assing VPN ip addresses to the clients. 'IP->DHCP Server'
![IPDHCPSERVER](https://user-images.githubusercontent.com/44748406/192097000-25147c4a-a62e-445a-a22b-682e6f744eab.png)
