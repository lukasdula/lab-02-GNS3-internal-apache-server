# **1 - Initial Preparation**

<br><br>

## **1.1 Introduction**

This note prepares the Xubuntu server for Lab 02 by installing the required packages while the **Cloud** node provides temporary internet access. The goal is to equip the server with essential services for client–server communication in an internal LAN: OpenSSH for remote access, Apache2 for HTTP service. Basic diagnostic tools are also installed to support testing throughout the lab. After installation and verification, the Cloud connection is removed and the lab continues in an internal network only.

![TYPOLOGY-map-1](images/Pasted%20image%2020251002004342.png)

<br><br>

## **1.2 Steps** with configuration

### **Xubuntu-1-server**

1. Connect Cloud for temporary internet access

Use _Cloud1_ only to reach the internet during installation. Disconnect it after verification.

2. Update packages

```bash
sudo apt update
```
![UPDATE](images/Pasted%20image%2020250930003514.png)

3. Install OpenSSH Server (remote access/management)

```bash
sudo apt install -y openssh-server
```
![INSTAL-SSH](images/Pasted%20image%2020250930004148.png)

* Enable SSH Service

```
sudo systemctl enable --now ssh
```
![Enable-SSH](images/Pasted%20image%2020250930004351.png)


4. Install diagnostic tools (network testing)

```bash
sudo apt install -y curl wget net-tools dnsutils tcpdump
```
![INSTAL-tools](images/Pasted%20image%2020250930005704.png)

- `curl`/`wget`: HTTP requests for quick checks
    
- `net-tools`: classic tools (ifconfig, netstat)
    
- `dnsutils`: includes dig for DNS queries (useful later)
    
- `tcpdump`: packet captures during troubleshooting
    

5. Install Apache2 (web service, HTTP/80)

```bash
sudo apt install -y apache2
```
![Instal Apache2](images/Pasted%20image%2020250930005845.png)

* Enable Apache2

```
sudo systemctl enable --now apache2
```
![Enable-Apache2](images/Pasted%20image%2020250930005943.png)


6. Install Bind9 DNS server

```
sudo apt install -y bind9
```
![Instal Bind9](images/Pasted%20image%2020250930010057.png)

## **1.3 Service verification**

### SSH service

```bash
systemctl status ssh --no-pager -l
```
![Veritifation-SSH](images/Pasted%20image%2020250930011204.png)

### Apache service

```bash
systemctl status apache2 --no-pager -l
```
![Veritifation-Apache](images/Pasted%20image%2020250930010438.png)

### Bind9

```
systemctl status bind9 --no-pager -l
```
![Veritifation-Bind9](images/Pasted%20image%2020250930010530.png)

At the end, verify that Apache is running by requesting `curl http://localhost`. **The output confirms that Apache is working.**

<br><br>

## **1.4 Conclusion**

The server now runs OpenSSH, Apache2, with essential diagnostic tools installed. The environment is ready for the next notes (network configuration and client communication). After installation and verification, the **Cloud** node is no longer needed for the demonstration and purposes of this lab; continue in an internal network only.


---

**Next part**: basic-network-configuration

























































































