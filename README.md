# Lab Apache Communication

## Introduction and Objectives

This second lab in GNS3 demonstrates the deployment of an Apache web server on Xubuntu and client access within an internal LAN.

The objectives are:

- Configure static IP addresses for the server and clients.
    
- Verify basic connectivity between devices (ping).
    
- Establish SSH access between clients, server, and router.
    
- Install and run Apache2 on the server.
    
- Replace the default page with a custom web page.
    
- Test HTTP access from clients using curl and a web browser.
    
- Capture and analyze traffic with tcpdump.
    

This lab builds on Lab 01 by extending the network functionality with a real application service. It demonstrates how clients interact with a web server and how to verify both connectivity and service availability.



![](Pasted%20image%2020251003012203.png)


## Lab Structure

1. Initial Preparation**
    
2. Basic Network Configuration**
    
3.  Apache Client Communication

## Key Features

- Static IP addressing and connectivity checks.
    
- SSH access between clients, server, and router.
    
- Apache2 service setup and verification.
    
- Custom index.html deployment.
    
- Client access demonstration with curl and browser.
    
- Basic diagnostics with tcpdump.
    

## Author's Note

This lab was my first hands-on test with Apache2 in GNS3. It was rewarding to see the clients successfully load the custom page from the server after replacing the default Apache page. I also experimented with tcpdump to watch HTTP traffic flow, which gave me a deeper understanding of network communication.

Combining SSH, IP configuration, and Apache testing made this lab feel like a complete small-scale environment, and it showed how multiple services interact in practice.

---


© 2025 – Lukas Dula | Home Network Lab & Portfolio