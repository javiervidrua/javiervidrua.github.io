---
layout: post
title:  "How to protect your SSH server"
date:   2020-11-15 00:00:00 +0200
categories: jekyll update
---
If you have a public SSH server and it has been running for some time, you probably have found that random IP addresses have tried to login, let's say 10000 times.

Those are hackers trying to get into your server.

There are various ways to protect your server against bruteforce attacks:

* **Have a strong password**

  As Kevin Mitnic says, you should not use passwords, but passphrases.
  
  That is sentences with 25 characters or more, including upper and lowercase letters and symbols.

*  **Change the default SSH port**

   Change the listening port to one that is currently unassigned by the [IANA](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml), for example, 22022.

   Note: This won't stop a hacker, since it is really easy to find the SSH service if you do a complete port scan.

*  **Install Fail2ban**

   On a Debian-based distribution, you can do so by running:
   ```bash
   sudo apt install fail2ban
   sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
   sudo nano /etc/fail2ban/jail.local
   ```

   In the file named `/etc/fail2ban/jail.local` you will be able to configure the installation to your liking.

   And when you're done, run this command to restart the service and apply the changes:
   ```bash
   service fail2ban restart
   ```

   You can manually ban or unban IP addresses too:
   ```bash
   sudo fail2ban-client set <jail> banip/unbanip <ip address>
   # For example
   sudo fail2ban-client set sshd unbanip 74.47.129.56
   ```

   Note: This is more likely to stop a hacker than the previous method.

*  **Install Endlessh**
   
   Before doing this you must have done the first option, that is, changing the default listening port of the SSH service.
  
   Endlessh is a tool that is built to run on port 22, pretending to be a real SSH service, but in reality what it will do is send an endless, infinte banner when someone tries to login.

   So **when a hacker tries to hack your server by bruteforcing the SSH service, he won't even be able to try one `user:password` combination**.

   To install it run the following commands:
   ```bash
   sudo apt install libc6-dev
   git clone https://github.com/skeeto/endlessh
   cd endlessh
   make
   sudo mv endlessh /usr/local/bin/
   ```

   Now we have to copy the .service file to the systemd directoy so it runs when the server starts, and enable the service:
   ```bash
   sudo cp util/endlessh.service /etc/systemd/system/
   sudo systemctl enable endlessh
   ```

   Then, we can create the config file:
   ```bash
   mkdir -p /etc/endlessh
   sudo echo 'Port 22' > /etc/endlessh/config
   ```

   Now, start endlessh:
   ```bash
   sudo service endlessh start
   ```

   We can now check if it is really running:
   ```bash
   root@torre:~# netstat -ano | grep 22
   tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      off (0.00/0/0)
   tcp6       0      0 :::22                   :::*                    LISTEN      off (0.00/0/0)
   root@torre:~#
   ```

   So now, when someone sees that you have port 22 open and tries to login, it will be endless and your server will be safe.

## Conclusion
These are some ways to quickly secure your SSH server and make it more robust.

If you only change the default port or install Endlessh, it won't stop a hacker, but if you implement all, your SSH server will be much more secure and less likely to get hacked.