---
title: "Make a Local Email Spam Filter"
date: 2024-02-17T20:29:30+05:30
draft: false
description: In this we will be making a email spam filter using a raspberry pi
tags: [blog, cybersecurity, foss, tech, tutorial]
tableofcontent: true
---

In this we will be creating a spam filter using a Raspberry Pi
to remove all the incoming spam messages.

<div class="dsclmr">
This assumes that you already have a fully working
Raspberry Pi with any of the Linux distros available
with Systemd as an init service.
</div>


> **Note:**
>
> *This only works for unmonitored clients such as thunderbird and other offline FOSS applications. I have not tried it on online email clients like Gmail and other proprietary spying websites.*



So, all that asides let's make a spam filter that filters emails
every 10 minute.

When it determines something as Spam, it creates an email
with the Spam attached in the Spam folder. ISBG fills
the email body with the assessment results and attaches
the SPAM to the new email (disarm nasty things like tracking).

### Install Spam-Assassin and enable it to run as a service
```sh
sudo apt-get install spamassassin
sudo systemctl enable spamassassin.service
```

### Check
```sh
sudo service --status-all #spamassasin should be in that list.
```

### Configure Spam-Assassin
In order to mark spam email with `*****SPAM*****` in
the subject, go to the `/etc/spamassassin/local.cf`

Uncomment the part that does that and make a change to
contact and hostname information (system-wide setting):
```sh
# Add *****SPAM***** to the Subject header of spam e-mails

rewrite_header Subject *****SPAM*****
report_contact isbg@SpamPi.net
report_hostname SpamPi.net
```
Then go to the local pi user directory and find
`/home/doomgate/.spamassassin/user_prefs`. This helps prevents
changes due to upgrades of Spamassassin You can set the
scoring for SPAM a bit more aggressive: 
```sh
# Set the threshold at which a message is considered spam (default: 5.0)

required_score 2.8
```

Also added whitelists and discovered not to use `"` or
`,` signs. I gave the example for blacklisting in commented
style:

```sh
# Whitelist and blacklist addresses are now file-glob-style patterns, so
# "friend@somewhere.com", "*@isp.com", or "*.domain.net" will all work.
# Added note do not use the quotes and comma and multiple lines with keyword are allowed
whitelist_from someone@coldmail.com news@somecompany.com
whitelist_from transactions@notice.somecompany.com
#blacklist_from thebad@badhost.com
```

### Installing ISBG.
Make sure you have pip3 installed (corresponding with Python3 which we check as well). Cause we will be installing this python3 package called [isbg](https://pypi.org/project/isbg/).
```sh
sudo apt-get install python3
sudo apt-get -y install python3-pip
sudo pip3 install isbg
```

Almost there...

Actually we are done... Try running it:
```sh
isbg --help
```

OK, just to get an idea of your IMAP structure run this
to get a list (*Note: you can append the `--savepw` to
have `isbg` remember your password in a local obfuscated
file. My Raspberry is not seen from the internet but
think twice about your setup here*)

```sh
isbg --verbose-mails --imaphost <<Provider_IMAP_HOSTname>> --imapuser <<USER_as_you_would_logon_in_webmail>> --imapport <<YourISPKnows>> --imaplist
```

Since I use email on my mobile device in POP3 mode
(I like a mail archive while on the road) and my desktop
in POP3 mode, I figured it would be best to have the
output written to my Spam folder and create an extra
IMAP account of the existing email account on my mobile
device. From each email address I now have an IMAP version.
That way I can monitor the IMAP 'Spam' folder. If something
gets caught as SPAM that should not be there, I can read
it, forward it to an unmonitored POP3 email box and also
make changes to the settings of SpamAssassin on the Raspberry.


### Configuring done. Time to setup a cron-job that detects spam messages every 10 minutes.

```sh
crontab -e
```

Does the rest. Do not forget to mark your script as executable AND include a PATH variable in the Crontab.
So this is my Crontab:
```sh
# Set PATH variables in this crontab
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games
#
# m  h dom mon dow   command
*/10 * * * * bash /home/doomgate/isbg/isbg_washer.sh
```

Also for testing purposes I added a rule in the
`user_prefs`:
```sh
# Added test to provoke a message to become SPAM for testing purposes
body LOCAL_MAKE_SPAM_RULE    /\bThis triggers it\b/i
score LOCAL_MAKE_SPAM_RULE  101.1
describe LOCAL_MAKE_SPAM_RULE     if text is seen then message is SPAM
```

Also. I like the SPAMhaus list and think the default
scores are a bit low. Override them in `user_prefs` if
you like:
```sh
# UPGRADING scores of SPAMHaus listing
score RCVD_IN_PBL 5.0
score RCVD_IN_XBL 5.0
score RCVD_IN_SBL 5.0
score RCVD_IN_CSS 5.0
```
Finishing my setup here's a small script with commands that runs all the code required.

```sh
#!/bin/bash
#exec &>/home/pi/isbg/cronjob.log    # you could uncomment this to look at CRON output if something is not working
isbg --imaphost <<Provider_IMAP_HOSTname>> --imapuser <<USER_as_you_would_logon_in_webmail>> --imapport <<YourISPKnows>> --partialrun 10 --spaminbox Spam --delete --expunge
```
