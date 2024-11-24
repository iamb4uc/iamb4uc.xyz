---
title: "Lets Make A VPN (StrongSwan)"
date: 2024-11-24T22:37:56+05:30
draft: false
description: 
tags: [blog]
---

![](https://github.com/iamb4uc/wallpapers/blob/main/gruvbox/gruv-room.png?raw=true)

In today's digital landscape, securing your online connections is more crucial
than ever. One effective way to achieve this is by setting up a Virtual
Private Network (VPN) using StrongSwan, an open-source tool that implements
the Internet Key Exchange (IKEv1 and IKEv2) protocols. This guide will walk
you through the process of installing and configuring a StrongSwan VPN
server on Ubuntu 24.04, along with instructions for connecting clients from
various operating systems.

## Why Choose StrongSwan?
StrongSwan serves as a keying daemon, allowing for encrypted connections
between hosts. By establishing a VPN, you can securely access resources on your
server and its network. This guide will not only cover the server setup but
will also detail how to connect clients running Ubuntu, Windows, and macOS.

## Prerequisites

Before you begin, ensure you have the following:

- An Ubuntu 24.04 server. If you're using Linode, you can sign up to receive a $100 credit for your first 60 days.
- Basic knowledge of using the terminal and the `sudo` command.
- A standard user account with SSH access configured securely.

## Installing StrongSwan on Ubuntu 24.04

### Step 1: Update Your System

Start by updating your package lists and upgrading installed packages:

```bash
sudo apt-get update && sudo apt-get upgrade
```

### Step 2: Install StrongSwan

SSH into your server and install StrongSwan along with necessary plugins:

```bash
sudo apt install strongswan strongswan-pki libcharon-extra-plugins libcharon-extauth-plugins libstrongswan-extra-plugins libtss2-tcti-tabrmd0 -y
```

### Step 3: Generate Server Keys and Certificates

Generate your IPsec private key and root certificate:

```bash
sudo ipsec pki --gen --size 4096 --type rsa --outform pem > /etc/ipsec.d/private/ca.key.pem
```

Create and sign the root certificate:

```bash
ipsec pki --self --in /etc/ipsec.d/private/ca.key.pem --type rsa --dn "CN=<Your VPN Server Name>" --ca --lifetime 3650 --outform pem > /etc/ipsec.d/cacerts/ca.cert.pem
```

Next, generate the VPN server's private certificate:

```bash
ipsec pki --gen --size 4096 --type rsa --outform pem > /etc/ipsec.d/private/server.key.pem
```

You will also need to create the host server certificate. Depending on your setup, you can use either a local resolver or a static IP address.

### Step 4: Configure StrongSwan

Enable packet forwarding by editing `/etc/sysctl.conf` and adding the following lines:

```plaintext
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
```

Next, configure StrongSwan by editing the `/etc/ipsec.conf` file and adding the necessary connection parameters. Here’s an example configuration:

```plaintext
config setup
    charondebug = "ike 1, knl 1, cfg 0, net 1"
    
conn ipsec-ikev2-vpn
    auto = add
    keyexchange = ikev2
    left = %any
    leftcert = server.cert.pem
    leftsendcert = always
    right = %any
    rightauth = eap-mschapv2
```

### Step 5: Authentication and Access Secrets

Create the `/etc/ipsec.secrets` file to handle user authentication:

```plaintext
: RSA "/etc/ipsec.d/private/server.key.pem"
username : EAP "<user’s password>"
```

Your StrongSwan server is now set up to accept client connections. Use the following command to check the status of your IPsec tunnel:

```bash
sudo ipsec status
```

## Connecting Clients to the StrongSwan VPN

### Ubuntu Client Setup

To connect from an Ubuntu client, install StrongSwan:

```bash
sudo apt install strongswan libcharon-extra-plugins
```

Download the server's certificate and configure the `ipsec.conf` and `ipsec.secrets` files similarly to the server setup.

### Windows 10 Client Setup

1. Import the VPN root certificate using the Microsoft Management Console (MMC).
2. Set up a new VPN connection through the Network and Sharing Center, entering the server's DNS name or IP address along with your credentials.

### macOS Client Setup

1. Download the server's CA certificate and import it into Keychain Access.
2. Set up a new VPN connection in System Preferences, providing the necessary server details and credentials.

## Troubleshooting Tips

Common issues often stem from mismatched credentials between the server and client. Additionally, firewall settings may block connections, so ensure these are correctly configured. For further diagnosis, check the StrongSwan log file located at `/var/log/syslog`.
