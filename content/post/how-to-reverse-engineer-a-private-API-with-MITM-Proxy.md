---
title: "How to Reverse Engineer a Private API With MITM Proxy"
date: 2023-11-14T22:49:27+05:30
draft: false
description: 
tags: [blog]
---

<div id="disclaimer" style="text-align: center; color: red;"><b>Disclaimer: This is for educational purposes, do not abuse this method.</b></div>

Developers can often struggle to find the data they need for their personal projects due to certain services locking down their API’s.

So I wanted to show you a way you can get the data you need.


## What is MITM Proxy and how does it work?

**MITM** stands for Man-in-the-Middle and mitmproxy is exactly that. It allows you to send data through a proxy that acts as a man-in-the-middle so you can read the request and responses being sent to and from the server.

![MITM Diagram](https://securebox.comodo.com/theme/images/man-in-the-middle-attack.png) 

## Before we begin

We’ll be installing this on a Macbook and capturing the data sent from an iPhone. However mitmproxy supports multiple platforms and devices. You can find out more in their [documentation](https://docs.mitmproxy.org/stable/).

## Installation

Head on over to [mitmproxy.org](https://mitmproxy.org) and follow their installation instructions. You should be able to open mitmproxy from the terminal with:

```bash
$ mitmweb
```

`mitmweb` is the GUI version that should open up in a new tab once it’s running.


## With mitmweb running we can configure our device

You are going to need the local IP address of the computer running mitmproxy. You can [Google](https://www.google.com/search?q=how+to+find+local+ip+address&rlz=1C5CHFA_enNZ909NZ909&oq=How+to+find+local+IP+address&aqs=chrome.0.0l8.3270j0j7&sourceid=chrome&ie=UTF-8) how to find this on your computer.

On your device follow these instructions: (Detailed tutorial in video above)

1. Enable the proxy setting on your devices connection sections.
2. Point the IP address to the local IP address of your computer
3. Set the port number to 8080
4. Authentication is not needed
5. Open your mobile browser (iPhone must be Safari)
6. Go to mitm.it
7. Click to download the certificate for your device type
8. On iPhone you then need to visit the setting page. You will be prompted about the recently downloaded certificate. Confirm the installation of the certificate there.

Bypassing iPhone’s extra security

1. Go to Settings -> General -> About -> Certificate Trust Settings
2. Enable full trust of root certificates.

## Setup is done, on to the fun stuff

Now you should be able to open up apps on your device and you will start seeing traffic popup in mitmweb

![img1](/img/MITM1.png) 

In our example we are capturing the data being sent to [Thingivese](https://thingiverse.com/) from the mobile app. If you look closely on the right hand side you will find the authentication header. In this header you will find the token being used to validate you as a user.

![img2](/img/MITM2.png) 

This is the token we can now use to query the API ourselves in our own personal application.

## Now try query the API yourself.
Here we are pulling the latest listings using Axios and sending the bearer token along with it.

```javascript
const http = require("http");
const Axios = require("axios");

http
  .createServer(function (req, res) {
    const url = "https://api.thingiverse.com/featured?page=1&per_page=10&return=complete";
    const token = "5a2d072366d7039776e4c35c5f32efaf";

    Axios.get(url, {
      headers: {
        Authorization: `token ${token}`,
      },
    })
      .then((result) => {
        res.setHeader("Content-Type", "application/json");
        res.end(JSON.stringify(result.data));
      })
      .catch((error) => {
        console.error(error);
      });
  })
  .listen(3000); //the server object listens on port 3000
```

## Worth noting

This may will not work for all applications as they may use ‘Certificate Pinning’ to which you’ll need an Android or Jailbroken iOS device.

You can read more about Certificate Pinning in the docs: [Certificate Pinning](https://docs.mitmproxy.org/stable/concepts-certificates/#certificate-pinning)
