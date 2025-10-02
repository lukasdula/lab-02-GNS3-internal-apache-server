# 3 - Apache Client Communication

## 3.1 Introduction

This final chapter verifies basic client communication with the Apache2 web server in the internal LAN. The server is reached by IP address, the default web page is opened, and then replaced with a simple custom page. Clients test access with `ping`, `curl`, and a web browser. Optional checks with `tcpdump` capture traffic.

![TOPOLOGY-map-3](images/Pasted%20image%2020251003010552.png)

## 3.2 Steps

1. Check Apache2 status on the server.
    
2. Request the default page from the server and from clients.
    
3. Replace the default page with a simple HTML file.
    
4. Test again from the clients.
    
5. Capture HTTP packets with `tcpdump`.
    
6. Verify communication.
    

## 3.3 Server –> check service and local access

In this part, the Apache2 service on the server is verified. The goal is to confirm that the service is active and listening on port 80. A quick test with `curl` shows that the default web page is reachable locally.

#### On Server (Xubuntu-1-Server):

```plaintext
systemctl status apache2 --no-pager -l
systemctl is-enabled apache2
```
![apache2-enabled](images/Pasted%20image%2020251002234045.png)

#### Document root:

The document root `/var/www/html` is the directory where Apache serves web files. It is not a command.

```plaintext
/var/www/html
```


#### Quick local test:

```plaintext
curl http://localhost
curl http://192.168.10.10
```
![local-1](images/Pasted%20image%2020251002234835.png)

```
curl http://192.168.10.10
```
![local-2](images/Pasted%20image%2020251002234921.png)

The Apache default page is displayed (full HTML output is not shown due to length).

## 3.4 Server -> change the default page

Here, the default Apache page is replaced with a simple custom message. This makes it easier to verify that clients really reach the server and not some cached content.

#### Edit the file (Xubuntu-1-Server):

```plaintext
sudo nano /var/www/html/index.html
```


#### Insert content:

```plaintext
<html><body><h1>Lab Web Server</h1><p>This page comes from Xubuntu-1-Server (192.168.10.10).</p></body></html>
```
![insert](images/Pasted%20image%2020251003000049.png)

#### Check permissions:

```plaintext
ls -l /var/www/html/index.html
```
![check](images/Pasted%20image%2020251003000130.png)

#### Reload Apache:

```plaintext
sudo systemctl reload apache2
```
![reload-apache](images/Pasted%20image%2020251003000153.png)

#### Test again:

```plaintext
curl http://192.168.10.10
```
![test-again](images/Pasted%20image%2020251003000219.png)

The new custom page is displayed.

## 3.5 Client tests -> curl and browser

In this section, both clients test connectivity to the server. Pings confirm IP reachability and `curl` confirms HTTP service availability. A web browser finally displays the page in a graphical way.

#### On Client-2 (Xubuntu-Client-2):

```plaintext
ping 192.168.10.10
curl http://192.168.10.10
```
![client-2-curl](images/Pasted%20image%2020251003002825.png)

#### On Client-3 (Xubuntu-Client-3):

```plaintext
ping 192.168.10.10
curl http://192.168.10.10
```
![ping-curl-client-3](images/Pasted%20image%2020251003002907.png)

#### Browser test (Client-3):

- Open 

```
http://192.168.10.10`
```
    

![open-browser](images/Pasted%20image%2020251003003223.png)

custom page loads.

## 3.6 Optional -> packet capture

This optional step captures HTTP packets on the server. It helps confirm that requests from clients really reach the server’s network interface.

#### On Server (Xubuntu-1-Server):

```plaintext
sudo tcpdump -i any -n 'tcp port 80'
```
![packet-capture-1](images/Pasted%20image%2020251003003520.png)

* Stop with `Ctrl+C`.

#### ICMP capture:

```plaintext
sudo tcpdump -i any -n icmp
```
![packet-capture-2](images/Pasted%20image%2020251003003717.png)



## 3.7 Diagnostics

This section lists the most common issues and how to check them if the page does not load.

#### On Server (Xubuntu-1-Server):

1. IP settings:
    

```plaintext
ip addr show dev ens3
ip route show
```
![test-1](images/Pasted%20image%2020251003005003.png)

2. Apache status and port:
    

```plaintext
systemctl status apache2 --no-pager -l
ss -lntp | grep ':80'
```
![test-2](images/Pasted%20image%2020251003005104.png)

3. Firewall:
    

```plaintext
sudo ufw status
sudo ufw allow 80/tcp
```
![test-3](images/Pasted%20image%2020251003005137.png)

    
4. File check:
    

```plaintext
ls -l /var/www/html/index.html
sudo systemctl reload apache2
```
![test-4](images/Pasted%20image%2020251003005459.png)
#### On Client-3:

5. Connectivity:
    

```plaintext
ping 192.168.10.10
```
![test-5](images/Pasted%20image%2020251003005608.png)



## 3.8 Conclusion

Apache2 runs correctly on the Xubuntu server. The default page is replaced with a custom page, and clients successfully reach it by IP. Tests with `ping`, `curl`, and a web browser confirm that communication works as expected. Optional packet captures show the HTTP traffic arriving at the server. With this chapter, Lab 02 is complete and we have learned the basics of Apache server operation and related functions.

---


**Back to project overview:** README
