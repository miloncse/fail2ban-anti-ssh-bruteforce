# Fail2ban custom jail configuration to protect SSH from bruteforce login attempts
This document provides instructions for configuring Fail2ban in ubuntu server to keep SSH password login enabled and protect server from bruteforce login attempts using simple custom jail configuration.

#### Update the package index:
  `sudo apt-get update`
#### Install Fail2ban
   `sudo apt-get install fail2ban`
#### Open the Fail2ban jail configuration file:
   `sudo nano /etc/fail2ban/jail.local`
#### Add the following configuration for the SSH jail to ban any machine (ip address) after failed attempts to bruteforce your server:
    [DEFAULT]      
    bantime = -1  
    [sshd]  
    enabled = true  
    port = ssh  
    filter = sshd  
    logpath = /var/log/auth.log  
    maxretry = 1  
    banaction = iptables-multiport  
#### `maxentry = 1` indicates a single failed login attempt should be considered for ban, increase the value if you want or you can white list your ip or multiple ips from this policy,
#### to white list a single or multiple ip's do follwing:
   `[DEFAULT]`  
   `ignoreip = 192.168.0.1 10.0.0.2 172.16.0.0/24`  
#### In the example above, three IP addresses are whitelisted: 192.168.0.1, 10.0.0.2, and the entire subnet 172.16.0.0/24. If you want to whitelist a single ip then just keep that single ip as ignoreip value  
#### Now Open the SSH filter configuration file:
    sudo nano /etc/fail2ban/filter.d/sshd.conf
#### Locate [Definition] section (the section should already be there if not add it), Add the following failregex pattern: 
   `failregex = ^(?:Failed password for (?:root|invalid user \S+)|Invalid user \S+) from <HOST> port \d+ ssh2`
#### if there is already a failregex entry, you can comment it by puttin #before the line or you can replace entire failregex with above failregex pattern given, this regex will prevent any invalid user or wrong password ssh login attempts from retrying again:
#### Restart the Fail2ban service to apply the changes and check if everything ok, you should see fail2ban running after above modificatoins:
    sudo systemctl restart fail2ban  
    sudo systemctl status fail2ban
#### View the current banned IP addresses in the sshd jail:
    sudo fail2ban-client status sshd
#### To unban a specific ip address put following:
    sudo fail2ban-client set sshd unbanip BANNED_IP_ADDRESS
##### Thank you! :)
