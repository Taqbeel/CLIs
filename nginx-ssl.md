# How To Install Nginx on Ubuntu 20.04+ and SSL configuration

In this guide, we’ll discuss how to install Nginx on your Ubuntu 20.04+ server, adjust the firewall, manage the Nginx process, and set up server blocks for hosting more than one domain from a single server.

**NOTE:** You will also optionally want to have registered a domain name before completing the last steps of this tutorial.

#### Step 1 – Installing Nginx
Because Nginx is available in Ubuntu’s default repositories, it is possible to install it from these repositories using the apt packaging system.

Since this is our first interaction with the apt packaging system in this session, we will update our local package index so that we have access to the most recent package listings. Afterwards, we can install nginx:

    sudo apt update
    sudo apt install nginx
    
After accepting the procedure, apt will install Nginx and any required dependencies to your server.

#### Step 2 – Adjusting the Firewall (Optional)
Before testing Nginx, the firewall software needs to be adjusted to allow access to the service. Nginx registers itself as a service with **ufw** upon installation, making it straightforward to allow Nginx access.

List the application configurations that **ufw** knows how to work with by typing:

    sudo ufw app list

You should get a listing of the application profiles:

    Output
    Available applications:
      Nginx Full
      Nginx HTTP
      Nginx HTTPS
      OpenSSH

As demonstrated by the output, there are three profiles available for Nginx:

- **Nginx Full:** This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
- **Nginx HTTP:** This profile opens only port 80 (normal, unencrypted web traffic)
- **Nginx HTTPS:** This profile opens only port 443 (TLS/SSL encrypted traffic)
It is recommended that you enable the most restrictive profile that will still allow the traffic you’ve configured. Right now, we will only need to allow traffic on port 80.

You can enable this by typing:

    sudo ufw allow 'Nginx HTTP'

You can verify the change by typing:

    sudo ufw status

The output will indicated which HTTP traffic is allowed:

    Output
    Status: active
    
    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere                  
    Nginx HTTP                 ALLOW       Anywhere                  
    OpenSSH (v6)               ALLOW       Anywhere (v6)             
    Nginx HTTP (v6)            ALLOW       Anywhere (v6)

#### Step 3 – Checking your Web Server


      
