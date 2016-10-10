# Setting Up a Secure Virtual Private Server (VPS)

#### About this guide
This guide will walk you through the process of setting up a __Linux__-based virtual private server (VPS).  It assumes you are using a __Mac OS X__ local machine.  It is adapted from the following guides:
* http://planetcrypton.com/hardened-server
* https://www.youtube.com/watch?v=DbPDraCYju8
* https://gist.github.com/learncodeacademy/5850f394342a5bfdbfa4

The primary use case for the server we will set up in this guide is to help [set up a dash masternode](https://github.com/riongull/notes/blob/master/dash_masternode_setup.md).  The server can, however, be more generally used for various purposes.  You would just need to install the correct programs and configure the correct ports.  Some of the generally used ports are shown below for reference.  

For the dash masternode use case, the server you are about to set up *is* your masternode, *not* your local Dash-Qt wallet.  Your local Dash-Qt wallet (set of private and public key pairs) holds the masternode collateral payment, but all of the functions (and required uptime) related to running a masternode is being done on the remote server you are setting up.  Your dash funds are *not* stored on this server.

#### Using this guide
* Basic terminal usage
  * This [basic terminal reference page](https://github.com/riongull/notes/blob/master/terminal_notes.md) may help if you are new to using terminal/shell/bash
* Command prompt legend
  * this guide uses the command prompts below to indicate which machine you are running commands on:
  * `Local$ sample command` - run on your __local Mac OS X__ terminal
  * `VPS$ sample command` - run on your __remote virtual private server's__ terminal

## 1. Create a Linux virtual private server (VPS)
You may use your own hardware, or any 3rd party hosting service you like (e.g. AWS, Digital Ocean, Vultr, etc).  This guide uses Vultr, which is a provider widely recommended for hosting masternodes.

*Note: The Vultr link below is an affiliate link.  Signing up through it is a good way to express appreciation for this guide.  The current promotion also gives you a $20 credit after your first $10 and 30 days of activity*

1. [Create a Vultr account](http://www.vultr.com/?ref=6971315-3B)
2. After signing in to Vultr, deploy a new 'Vultr Cloud Compute (VC2)' instance
  * Location: somewhere near you
  * Server Type: 64 bit OS > Ubuntu > 14.04 x64
  * Server Size: 20 GB SSD, 1024 MB memory, 2000 GB bandwidth
  * No additional features are necessary
3. Click 'Deploy Now'
4. Wait for your new server to get to 'running' status
5. Copy your server's IP address and default password to another application like Word or Notes
  * These will be referred to as \<ip.add.re.ss> and \<defaultPassword> in later steps

## 2. Set up the VPS with new users

1. Log into the *remote* (newly created) VPS from your *local* computer's terminal

  ```sh
  Local$ ssh root@<ip.add.re.ss>
  <defaultPassword>
  ```
2. Change the 'root' user's password

  ```sh
  VPS# passwd root
  <newPassword> # this should be very secure
  <newPassword> # confirm password
  ```
3. Create a couple new users, give one sudo privileges  

  ```sh
  VPS# adduser <super-user>
  <newPassword>
  <newPassword>
  <otherFieldsAsDesired>
  VPS# sudo usermod -a -G sudo <super-user> # adds super-user to 'sudo' group
  VPS# adduser <login-user>
  <newPassword>
  <newPassword>
  <otherFieldsAsDesired>
  ```

## 3. Secure the VPS using...

*... either __passwords__ for logging in to the VPS, by doing the following:*

1. Create strong passwords for ALL users

  ```sh
  # 1.1 give login-user a secure password
  VPS# su <login-user>
  VPS$ passwd
  <newPassword>
  <newPassword>
  VPS$ exit
  # 1.2 give super-user a secure password
  VPS# su <super-user>
  VPS$ passwd
  <newPassword>
  <newPassword>
  VPS$ exit
  # 1.3 give root a very secure password if you haven't already
  VPS# passwd
  <newPassword>
  <newPassword>
  VPS# exit
  ```

*...or __SSH key__ for logging in to the VPS, by doing the following:*

1. Create SSH folder if needed

  ```sh
  Local$ cd ~
  Local$ ls
  # if there's a folder named .ssh, then skip the next step, otherwise:
  Local$ mkdir ~/.ssh
  ```
2. Create SSH keys

  ```sh
  Local$ pwd # make note of the full path to your present working directory, <fullPathOfPWD> for next step
  Local$ touch <fullPathOfPWD>/<dash-server-key> # <dash-server-key> is a descriptive name of your choice
  Local$ ssh-keygen -t rsa
  <fullPathOfPWD>/<dash-server-key> # enter the same file (with full path) that you entered above
  y # answer yes to overwrite file
  <really-long-and-hard-to-guess-passphrase>
  <really-long-and-hard-to-guess-passphrase>
  # set key permissions
  Local$ chmod 644 <fullPathOfPWD>/<dash-server-key>.pub
  Local$ chmod 600 <fullPathOfPWD>/<dash-server-key>
  ```
3. Create & edit `~/.ssh/config` to handle multiple keys

  ```sh
  Local$ touch ~/.ssh/config
  Local$ chmod 644 ~/.ssh/config
  Local$ nano ~/.ssh/config
  < # Start of contents of new config file
    Host <ip.add.re.ss> # this can be any alias, e.g. 'my-server' instead of 'ip.add.re.ss', you then log in with: 'ssh root@my-server'
      Hostname <ip.add.re.ss> # this can be either your 'ip.add.re.ss' or 'your-registered-domain.com'  
      PreferredAuthentications password # we will change 'password' to 'publickey' later
      IdentityFile ~/.ssh/<dash-server-key> # this will be needed when we use 'publickey' authentication
  > # End of contents of new config file, save and close
  ```
  * You may [add other entries to the file](https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client), including those previously set up to connect via shh
4. Copy & paste the public key from *local* to *remote* and set permissions

  ```sh
  Local$ cd ~/.ssh
  Local$ cat <dash-server-key>.pub | pbcopy
  Local$ ssh root@<ip.add.re.ss> # log into VPS
  <password>
  # You're now in your VPS...
  VPS$ mkdir /home/<login-user>/.ssh/
  VPS$ touch /home/<login-user>/.ssh/authorized_keys
  VPS$ nano /home/<login-user>/.ssh/authorized_keys
  [command+v] # press command+v to paste in the key you copied from local
  # Set the permissions.
  VPS$ chown -R <login-user>:<login-user> /home/<login-user>/.ssh
  VPS$ chmod 700 /home/<login-user>/.ssh
  VPS$ chmod 600 /home/<login-user>/.ssh/authorized_keys
  ```
5. Configure SSH on *remote* server

  ```sh
  # copy the config file just in case we screw things up while editing it, just in case.
  VPS$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_original

  # NOTE: this step changes your server to a very “strict” configuration.  
  # After this change you will ONLY be allowed to log-in to your REMOTE server from your current LOCAL machine.  
  # To be able to log-in from a different LOCAL machine you would need to copy the private ssh key from your LOCAL machine onto the other LOCAL machine.  (You might want to keep the private key on an encrypted USB flash drive for such purposes.)  If that other LOCAL machine were not also owned by you, then you would want to delete the private key from it after you were done using it.  If you were willing to compromise just a bit on security you could leave PasswordAuthentication set to yes; it would be better if you could avoid doing this, however, in the event someone guessed or otherwise found out <login-user>'s password.
  VPS$ sudo nano /etc/ssh/sshd_config

  # Check, set, or add the following lines in the sshd_config file:
  < #start of desired file settings
    Protocol 2
    PermitRootLogin no
    PasswordAuthentication no
    UseDNS no
    AllowUsers <login-user>
  > # end of desired file settings

  VPS$ service ssh restart # restart ssh.  
  VPS$ exit # log out so we can test our logging-in with our new configuration

  # before logging back into our server we must edit the ~/.ssh/config file to allow publickey log-ins
  Local$ nano ~/.ssh/config
  < # Start of contents of config file
    Host <ip.add.re.ss>
      Hostname <ip.add.re.ss>  
      PreferredAuthentications publickey # change 'password' to 'publickey'
      IdentityFile ~/.ssh/<dash-server-key>
  > # end of contents of config file
  Local$ ssh <login-user>@<ip.add.re.ss>
  <password>
  # if you can successfully log-in at this point, then you can continue on to the “Configuring Ports” section below.  If you cannot log-in, then you can try to go back and fix any problems by logging-in through a web-based console provided by your cloud-server's host.  If you just can't get it working no matter what, you may have to start again, rebuilding the server from scratch.
  ```

## 4. Configure VPS ports

1. Configure default access

  ```sh
  # as designed, we can only log in to our server using <login-user>, but to make administrative changes we must now sign in with <super-user> (because only it has sudo permissions)
  VPS$ su <super-user>
  # deny all incoming and allow all outgoing connections by default:
  VPS$ sudo ufw default deny incoming
  VPS$ sudo ufw default allow outgoing
  ```

2. Edit SSH port for TCP connections
  * [For security](https://major.io/2013/05/14/changing-your-ssh-servers-port-from-the-default-is-it-worth-it/), change your ssh-port-number for tcp connections, and open that port
  * Your chosen \<ssh-port-number> should be something non-standard, and ideally not used by [other services](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)
  ```sh
  VPS$ sudo ufw allow <ssh-port-number>/tcp
  ```

3. Configure desired ports (common ports shown)

  ```sh
  # you may now open a number of commonly used ports. You may not need some of these ports, or be unsure as to which you do or do not need.  For most configurations, opening the ports shown below should be safe.  If you are sure that you do not need to open some port, feel free to skip that step.  Also if you wanted to close a port later on, you could to this by simply issuing the command: sudo ufw deny <port>/<optional: protocol>.  For example, to close port 53 for everything: sudo ufw deny 53. To deny incoming tcp packets to port 53: VPS$ sudo ufw deny 53/tcp. To deny incoming udp packets to port 53: VPS$ sudo ufw deny 53/udp.  
  # commands to open commonly used ports/protocols:
  VPS$ sudo ufw allow 25/tcp # SMTP - Simple Mail Tranfer Protocol Transmission
  VPS$ sudo ufw allow 53/tcp # DNS - Dynamic Name Server (use both lines)
  VPS$ sudo ufw allow 53/udp
  VPS$ sudo ufw allow 80/tcp # HTTP - Hyper-Text Transfer Protocol
  VPS$ sudo ufw allow 110/tcp # POP3 - Post Office Protocol
  VPS$ sudo ufw allow 143/tcp # IMAP - Internet Mail Access Protocol
  VPS$ sudo ufw allow 443/tcp # HTTPS - Hyper-text Tranfer Protocol Secure
  VPS$ sudo ufw allow 465/tcp # SMTPS - Simple Mail Tranfer Protocol Secure
  VPS$ sudo ufw allow 587/tcp # SMTP - Simple Mail Tranfer Protocol Submission
  # commands to open commonly used ports/protocols related to Dash
  VPS$ sudo ufw allow 7903 # P2Pool-dash, needed if you are running a Dash P2Pool
  VPS$ sudo ufw allow 9999/tcp # Dash port 9999:mainnet, needed if you are running Dash on the standard main network, both lines
  VPS$ sudo ufw allow 9999/udp
  VPS$ sudo ufw allow 19999/tcp # Darkcoin Port 19999:testnet, needed if you are running Dash on the testing network, both lines
  VPS$ sudo ufw allow 19999/udp
  ```

4. Re-edit `/etc/ssh/sshd_config`  ### RDG Note: this step is not applicable ("re-edit") to users who didn't do ssh login

  ```sh
  # edit the ssh configuration file again, just to change the Port to the <ssh-port-number> you chose above:
  VPS$ sudo nano /etc/ssh/sshd_config
  < # just change the one line:
    Port <ssh-port-number>
  >
  ```

5. Enable UFW, reboot VPS, log back in from Local

  ```sh
  VPS$ sudo ufw enable # you've made changes, now enable the firewall (the UFW)
  y # confirm enable
  VPS$ sudo ufw status numbered # check its status (you can omit the word “numbered,” but it provides more information)
  VPS$ sudo reboot # reboot the server,
  # you should now be able to log back in using our ssh private key and ssh passphrase, now also including the new <ssh-port-number> in the login. You may have to wait about a minute or so for it to boot up before you can login
  Local$ ssh -p <ssh-port-number> <login-user>@<ip.add.re.ss> # log back in, if it works, continue to the next section, “Update and Upgrade and Install General-Dependencies.”  If it does not work, you may have to rebuild from scratch, unless you can log-in via a web-console provided by your cloudserver host to try to fix the problem.
  ```

## 5. Update, upgrade, & install general VPS dependencies

1. Switch to super-user, and super-user's home directory and update the package list

  ```sh
  VPS$ su <super-user>
  VPS$ cd ~
  VPS$ sudo apt-get update
  VPS$ sudo apt-get upgrade
  VPS$ sudo apt-get dist-upgrade
  ```

2. Install general dependencies/packages, reboot, and log back in

  ```sh
  VPS$ sudo aptitude install git build-essential screen curl mailutils
  VPS$ sudo reboot # as before, you may have to wait a very short time for it to boot up before you can login.)
  VPS$ ssh -p <ssh-port-number> <login-user>@<ip.add.re.ss>
  VPS$ su <super-user>
  VPS$ cd ~
  ```

You're done setting up your secure, "hardened" Linux server!
