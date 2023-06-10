# Fail2ban Configuration

This document provides instructions for configuring Fail2ban in ubuntu to protect SSH from bruteforce login attempts using a simple custom jail configuration.

#### Update the package index:

`sudo apt-get update`

#### Install Fail2ban

` sudo apt-get install fail2ban`

#### Open the Fail2ban jail configuration file:

`sudo nano /etc/fail2ban/jail.local`

#### Add the following configuration for the SSH jail to ban any machine (ip address) after trying 3 failed attempts to bruteforce your server ssh in a day:

  `[sshd]
   enabled = true
   port = ssh
   filter = sshd
   logpath = /var/log/auth.log
   maxretry = 3
   bantime = -1
   findtime = 1d`

#### Now Open the SSH filter configuration file:

`sudo nano /etc/fail2ban/filter.d/sshd.conf`

#### Add the following failregex pattern under [Definition] section (the section should already be there if not add it), also if the failregex already exits comment it or replace it by following, this regex will prevent any invalid user or wrong password/public key ssh login attempts:

`failregex = ^(?:\S+ )?(?:<[^.]+\.[^.]+>)?\s*(?:\S+ )?(?:\S+ )?\s*Failed (?:password|publickey) for invalid user \S+ from <HOST>\s*$`

#### Restart the Fail2ban service to apply the changes and check if everything ok, you should see fail2ban running after above modificatoins:

`sudo systemctl restart fail2ban
 sudo systemctl status fail2ban`

#### View the current banned IP addresses for the SSH jail:

`sudo fail2ban-client status sshd`

#### To unban a specific ip address put following:

`sudo fail2ban-client set sshd unbanip BANNED_IP_ADDRESS`

##### Thank you! :)
