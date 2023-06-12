---
title: "Lets Join Tilde Varsh"
date: 2022-09-16T22:48:40+05:30
draft: false
tags: [blog, foss, tutorial, tech]
---
Few weeks ago I came across this thing called a tilde on the [gnulinuxindia.org](https://gnulinuxindia.org) website, while lurking around the web. I had zero idea about what it was about or what even is it. So in their terms it is
> An India-based shared unix system which focuses on mutual development of skills and sharing of ideas. Services are secondary in this tilde, community is prime.
>
> --<cite> Tildevarsh</cite>

Still confused watch this [Video](https://yewtu.be/watch?v=qK1mInnbfrU)

If you want to join too you can fill up this form with your email and other details [here](https://tildevarsh.in/register) and have fun with your new UNIX system.

#### Step-By-Step Guide
1. Enter you username, email address of choice in the "Username" and the "You email (Need one so we can contact you):" field.
2. Enter you pre-existing ssh(public)key.
3. If you dont have one then lets create a new one using the following commands.
```bash
ssh-keygen
```

After that you would be prompted by the following:
```bash
Generating public/private rsa key pair.

Enter file in which to save the key (/home/username/.ssh/id_rsa):
```
This will create a new file and a directory in `$XDG_HOME_DIR/.ssh`. After that, you would be prompted for a password for a key. **It is very important that you make a hard password since it will be used for you to login to the machine and for any other future logins**  
After completing you will get some form of ASCII art for confirmation.  
Now copy and paste the contents of `$XDG_HOME_DIR/.ssh/id_rsa.pub` to the public key field in the form.

Finally after finishing wait for ~24hrs and you will receive an email with your login credentials.

After your first login with the default credentials, change your password for your account.

**REMEMBER DO NOT STORE ANYTHING PERSONAL IN THESE MACHINES AS IT IS A MULTIUSER SYSTEM**

You can create a free website in the `public_html` directory.
Also dont forget to visit my tilde at [~b4uc](https://b4uc.tildevarsh.in/)

