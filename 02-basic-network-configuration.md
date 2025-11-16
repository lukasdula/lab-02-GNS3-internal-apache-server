# **2 - Basic Network Configuration**

<br><br>

## **2.1 Introduction**


In this part of the lab, the internal LAN uses static IP addresses for the router, server, and clients. The router works as the default gateway, and SSH remote access is needed between all devices. Because Cisco IOSv supports only older cryptographic algorithms, which modern OpenSSH disables by default, the Xubuntu SSH server must be changed to accept these legacy methods. This makes sure the router can connect to the server even with a newer OpenSSH version.

![TYPOLOGY-map-2](images/Pasted%20image%2020251002000750.png)

<br><br>

## **2.2 Topology**

**The following devices are used in the topology:**

|Device|Interface|Connected to ->|Peer Interface|IP Address|Subnet Mask|Default Gateway|
|---|---|---|---|---|---|---|
|Router (R1)|Gi0/0|Switch SW1|Gi0/0|192.168.10.1|255.255.255.0|-|
|Xubuntu-1-Server|Gi0/0|Switch SW1|Gi0/1|192.168.10.10|255.255.255.0|192.168.10.1|
|Xubuntu-Client-2|Gi0/0|Switch SW1|Gi0/2|192.168.10.11|255.255.255.0|192.168.10.1|
|Xubuntu-Client-3|Gi0/0|Switch SW1|Gi0/3|192.168.10.12|255.255.255.0|192.168.10.1|

<br><br>

## **2.3 Steps**


- Configure router R1 interface Gi0/0 with gateway IP address.
    
- Configure SSH on router and functionality
    
- Assign static IP addresses to Xubuntu server and clients.
    
- Define default gateway on server and clients as 192.168.10.1.
    
- Verify connectivity with SSH between devices.
    
- Perform diagnostic tests using ping and SSH sessions.
    
<br><br>

## **2.4 Configuration**

### Router R1

```
configure terminal
hostname R1
interface gi0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
ip domain-name lab.local
crypto key generate rsa 
1024
ip ssh version 2
username admin secret admin12345
enable secret cisco12345
line vty 0 4
login local
transport input ssh
exit
write memory
```
![R1-CONFIGURATION](images/Pasted%20image%2020251001235018.png)

<br><br>

## **2.5 Assign static IP to Xubuntu PCs**
### Xubuntu-1-Server

```
sudo ip addr add 192.168.10.10/24 dev ens3
sudo ip route add default via 192.168.10.1
```
![xubuntu1-ip](images/Pasted%20image%2020251001005227.png)

* Verify ip and gateway

```
ip addr show dev ens3
ip route show
```
![verify-ip-gateway](images/Pasted%20image%2020251001010614.png)

### Xubuntu-Client-2

```text
sudo ip addr add 192.168.10.11/24 dev ens3
sudo ip route add default via 192.168.10.1
```
![xubuntu2-ip](images/Pasted%20image%2020251001005349.png)

### Xubuntu-Client-3

```text
sudo ip addr add 192.168.10.12/24 dev ens3
sudo ip route add default via 192.168.10.1
```
![xubuntu3-ip](images/Pasted%20image%2020251001190105.png)

<br><br>

## **2.6 SSH Access**

### SSH Compatibility Note

Modern Linux SSH clients do not accept some old algorithms used by Cisco IOSv routers. Because of this, a normal SSH command may fail. To connect from Xubuntu to the router, use this command:

```text
ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostKeyAlgorithms=+ssh-rsa -c aes128-ctr
```

#### Explanation

- `-oKexAlgorithms=+diffie-hellman-group14-sha1` → allows an older key exchange method.
    
- `-oHostKeyAlgorithms=+ssh-rsa` → accepts the older RSA host key.
    
- `-c aes128-ctr` → sets a cipher (encryption method) that IOSv can work with.


### Enable Legacy SSH Algorithms on Xubuntu

Modern OpenSSH disables older algorithms by default, while Cisco IOS uses only these legacy methods. To make the router connect successfully to the Xubuntu SSH server, the server configuration must allow these older algorithms. This is done by adding specific directives in the SSH configuration file using the commands shown below.

### Open the SSH server configuration file

1. `sudo nano /etc/ssh/sshd_config`

2. Add the following lines at the end of the file

```
KexAlgorithms +diffie-hellman-group14-sha1
HostKeyAlgorithms +ssh-rsa
Ciphers +aes128-ctr
```
![SHH-configuration-file](images/Pasted%20image%2020251001233015.png)


3. Restart the SSH service

* `sudo systemctl restart ssh`


**Now the connection from the Cisco router to the Xubuntu SSH server works successfully.**

--- 

### Verify SSH Connection

To verify remote management, SSH connections are tested between devices:

1. From server to router (R1):
    

```text
ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostKeyAlgorithms=+ssh-rsa -c aes128-ctr admin@192.168.10.1
```
![SSH-SERVER-to-R1](images/Pasted%20image%2020251001192156.png)

2. From router (R1) to server:    

```
ssh -l xubuntu1 192.168.10.10
```
![R1-to-server](images/Pasted%20image%2020251001233414.png)

3. From Server to Xubuntu-Client-2:
    
```
ssh xubuntu2@192.168.10.11
```
![SSH-Xubuntu-to-client-2](images/Pasted%20image%2020251001201307.png)

>Note.: The SSH client package is already preinstalled on Xubuntu, so there is no need to install it separately.

<br><br>

## **2.7 Diagnostics**

1. Verify connectivity from clients to server.
    
    - From client-2 to Server:
        
    
```
ping 192.168.10.10
```
![ping-client-2-to-server](images/Pasted%20image%2020251001234740.png)

	
	
* From Client-3 to server
	

```
ping 192.168.10.10
```
![ping-client-3-to-server](images/Pasted%20image%2020251001234828.png)

2. Verify connectivity between server and router.
    
    - From Server to Router (R1):
        

```
ping 192.168.10.1
```
![ping-server-to-R1](images/Pasted%20image%2020251001234859.png)

3. Verify conectivity from client-2 to client-3

```
ping 192.168.10.12
```
![ping-client-2-to-client-3](images/Pasted%20image%2020251001234927.png)

<br><br>

## **2.8 Conclusion**

The Xubuntu SSH server is now configured to support older cryptographic algorithms, so it is compatible with Cisco IOSv routers. With this change, SSH sessions can be created in both directions: from Xubuntu to the router and from the router back to Xubuntu. All devices can also ping each other, confirming that the basic connectivity works and every host in the LAN can reach the others. The lab environment is now fully functional for remote access and further tasks.


---

**Next part:** apache-client-communication

