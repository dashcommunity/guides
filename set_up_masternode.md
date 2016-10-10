# Setting Up a Dash Masternode

#### About this guide
This guide will walk you through the process of setting up a [dash masternode](http://dashmasternode.org/) using a __Mac OS X__ local machine and your own remote __Linux__ virtual private server (VPS).  It is adapted from the following guides:
* https://www.dash.org/forum/threads/masternode-setup-guide-using-os-x-local-linux-remote.1776/
* https://www.dash.org/forum/threads/taos-masternode-setup-guide-for-dummies-updated.2680/

#### Using this guide
* Basic terminal usage
  * This [basic terminal reference page](https://github.com/riongull/notes/blob/master/terminal_notes.md) may help if you are new to using terminal/shell/bash
* Command prompt legend
  * This guide uses the command prompts below to indicate which machine you are running commands on:
  * `Local$ sample command ` - run on your __local Mac OS X__ terminal
  * `VPS$ sample command` - run on your __remote virtual private server's__ terminal
  * `Dash$ sample command` - run on your __local Dash-Qt's__ console

## 1. Download & install Dash-Qt & prepare your wallet
The following steps will help you [install a dash wallet on your Mac OS X](https://node40.com/2016/02/26/how-to-install-and-secure-the-dash-qt-(core)-wallet.html).  Although it is a bit dated, [this video](https://www.youtube.com/watch?v=hCGZPN0Sb84&index=3&list=PLiFMZOlhgsYLWcmb-MT6x7cIxb01OoJTB) may help as well.

1. From a browser on your Mac, go to https://www.dash.org/downloads
2. Find and click the Dash Core, Mac OS X dmg download link
3. Click the downloaded dmg file to open up its contents
4. Drag the Dash-Qt.app file into the Applications folder
5. Open the Applications folder and press control while clicking Dash-Qt
6. From the menu shown, click 'Open', then confirm by clicking 'Open' again
7. Press 'OK' to install the app into the default directory
8. Select 'Deny' or 'Allow' depending on your preference
9. Note the status of the network sync shown at the bottom; this may take hours to complete but you can move on
10. While syncing, and before depositing any dash, [encrypt your wallet](https://www.youtube.com/watch?v=PcnAvExGLJs&index=8&list=PLiFMZOlhgsYLWcmb-MT6x7cIxb01OoJTB) and [back it up](https://node40.com/2016/03/30/how-to-backing-up-your-dash-public-key-by-dumping-the-corresponding-private-key.html)

## 2. If necessary, move `dashd` to the proper folder
Some older Dash-Qt installations saved `dashd` in the "downloads" folder. If you already had Dash-Qt installed, double check to make sure it is in the proper place, as follows:

1. Quit Dash-Qt (`command` + `q` when client is active window)
2. Open the terminal utility (`command` + `spacebar`, then type 'terminal')
3. Run the following commands:

  ```sh
  Local$ cd ~/Library/Application\ Support/Dash
  Local$ ls -la
  # if you don't see a file called dashd, run the following commands
  Local$ cd ~/Downloads
  Local$ mv dashd ~/Library/Application\ Support/Dash
  ```

## 3. Create and secure a Linux VPS
This step (done properly) requires a lot of steps, so there is a link below to a separate guide for this step.  Once complete, come back to this guide and continue on with step 4.  

1. Create and secure a virtual private server (VPS) to host your masternode
  * You may use your own hardware, or any 3rd party hosting service you like (e.g. AWS, Digital Ocean, Vultr, etc)
  * This [comprehensive VPS setup guide](https://github.com/riongull/notes/blob/master/virtual_private_server_setup.md) walks you through this process, using [Vultr](http://www.vultr.com/?ref=6971315-3B) as your VPS host
  * Your machine should have sufficient storage, bandwidth, and RAM, and be properly configured (ssh, ports, etc)

## 4. Create your masternode key & address
1. Open Dash-Qt
2. Get a new masternode private key and address
  * Menu: Tools > Debug console
  * A new window should appear with the "Console" tab selected at the top
  * Enter the following command in the input field at the bottom:
  ```sh
  Dash$ masternode genkey
  # Dash will output the result
  Dash$ getaccountaddress <masternodeLabel> # <masternodeLabel> is a name of your choice, e.g. "masternode1", "MN01", etc.
  # Dash will output the result
  ```
3. Copy and label the two outputted strings of characters in the notes page you created for your IP address
  * These will be referred to as \<masternode-privkey> and \<zero-address> in later steps

## 5. Fund your newly created address with 1000 DASH
You may fund your masternode address by sending dash from any wallet (e.g. Jaxx, Electrum, Poloniex, etc).  These steps assume you are funding from Dash-Qt itself.  Either way, the wallet you are sourcing funds from must have 1000 DASH plus a little extra (~0.01 DASH) to cover the dash network fee.

1. Open the 'Receiving addresses' dialog box (file > Receiving addresses)
2. Click the Receiving address you made in step 4 (e.g. "MN01") and click the 'copy' button
3. Click the Send tab on the main Dash-Qt window (or your wallet of choice)
4. Paste the address you copied on the 'Pay To' field, it should populate your label
5. Enter '1000' in the 'Amount' field
  * Don't include any miner fee in this amount; the wallet will automatically cover it
  * The amount you actually send must be one transaction of exactly 1000 DASH
  * No, you may not send more than 1000 DASH
  * No, you may not send 999.99 DASH
  * No, you may not put in 1 DASH first and then 999 DASH. It must be all at once. You may test the address first, but you will still need to send 1000 DASH later, all in one transaction
  * No, even if you have 1000 DASH in the wallet already, that doesn't count. You must send 1000 DASH to your `<masternodeLabel>` address
6. Click 'Send' to fund your masternode (you may use/check InstantSend for this)
7. Click 'Transactions' in the main Dash-Qt window.  Your transaction should show "Darksend collateral payment"

## 6. Prepare your remote VPS
While we are waiting for the needed 6 confirmations of our 1000 DASH transaction, we can now prepare the remote server.

1. Log in to your VPS

  ```sh
  Local$ ssh <login-user>@<ip.add.re.ss>
  <password> # if needed
  ```
  * If you changed your SSH port in step 3.5 of [securing your VPS](https://github.com/riongull/notes/blob/master/vitual_private_server_setup.md#3-secure-the-vps-using) you may need to:
  ```sh
  Local$ ssh -p <ssh-port-number> <login-user>@<ip.add.re.ss>
  ```
  * You may also log in from your VPS cloud provider's console

2. Optionally, create a new user on your VPS for all dash-related files and functions

  ```sh
  VPS$ su <super-user>
  <password>
  VPS$ sudo adduser <dash-user> # this will be the assumed target for '~' from now on (cd ~ == cd /home/<dash-user>)
  <super-users-password>
  <newPassword>
  <newPassword>
  <otherFieldsAsDesired>
  ```

3. Download, unpack, copy, and permission the needed applications/files on your VPS

  ```sh
  VPS$ su <dash-user> # whatever user you want; <dash-user> if you created it above
  VPS$ cd ~
  VPS$ wget https://www.dash.org/binaries/dash-0.12.0.58-linux64.tar.gz
  VPS$ tar xfvz dash-0.12.0.58-linux64.tar.gz # unpack files
  VPS$ cp dash-0.12.0/bin/dashd dashd
  VPS$ cp dash-0.12.0/bin/dash-cli dash-cli
  VPS$ chmod 755 dashd # set permissions
  ```
4. Create `dash.conf` file on your VPS

  ```sh
  VPS$ cd ~
  VPS$ mkdir .dash
  VPS$ cd .dash
  VPS$ nano dash.conf
  # once open, insert the following text.  Note: rpcuser and rpcpassword can be anything, you will not have to remember them later, feel free to go nuts on your keyboard

  < # start of file contents
  rpcuser=<anything-like-random-numbers-and-letters>
  rpcpassword=<anything-like-random-numbers-and-letters>
  rpcallowip=127.0.0.1
  listen=1
  server=1
  daemon=1
  logtimestamps=1
  maxconnections=256
  masternode=1
  masternodeprivkey=<masternode-privkey>
  > # end of file contents, ctrl+x to exit, save as "dash.conf"
  ```
5. Launch dashd on your VPS

  ```sh
  VPS$ cd ~
  VPS$ ./dashd # enter command, then wait ~15 seconds -- to stop dashd issue the command ./dash-cli stop  
  VPS$ ./dash-cli getinfo
  ```
  * If you run the "getinfo" command several times you should see that the number of blocks is increasing
  * The number of blocks must eventually catch up to the current blockchain before your masternode is active
  * You can check if the number of blocks is up to the current height by comparing to the current number of blocks reported on one of the many Dash blockchain explorers (several are listed on the dash.org website)
  * You don't need to wait for the blocks to completely sync; we may move on to the next step

## 7. Remove unnecessary files & folders from your VPS
We are done with the installation files and folders, so those can be removed.

1. Remove files as follows:

  ```sh
  VPS$ cd ~
  VPS$ ls
  VPS$ rm -rf dash-0.12.0
  VPS$ rm dash-0.12.0.58-linux64.tar.gz
  VPS$ exit # repeat to exit all the way out of VPS (into your local machine)
  ```

## 8. Create `dash.conf` & `masternode.conf` files on your *local* machine
1. Close Dash-Qt if open (command+Q to quit the program, not just close the windows)
2. Open (or switch back to) terminal
3. Create `dash.conf` file

  ```sh
  Local$ cd ~/Library/Application\ Support/Dash
  Local$ nano dash.conf # this should bring up a GNU session that is blank (unless you already had a conf file created). In any case, make sure it has the following text.
  < # start of file contents
  rpcuser=<anything-like-random-numbers-and-letters-probably-safer-to-use-something-different-than-the-last-one>
  rpcpassword=<anything-like-random-numbers-and-letters-probably-safer-to-use-something-different-than-the-last-one>
  rpcallowip=127.0.0.1
  listen=0
  server=1
  daemon=1
  logtimestamps=1
  maxconnections=8
  > # end of file contents, ctrl+x to exit, save as "dash.conf"
  ```

4. Obtain data for `masternode.conf`
  1. Easy Method:
    1. Open Dash-Qt
    2. From the menu: Tools > Debug Console, then type the command:

    ```sh
    Dash$ masternode outputs # values are listed as "hash":"index", copy them into your notes
    ```

  2. Alternate Method:
    1. Open Dash-Qt
    2. From the menu: File > Receiving addresses
    3. Clck on your masternode addresses and click copy
    4. Go to a dash block explorer, e.g. [chainz](https://chainz.cryptoid.info/dash/),
    5. Paste the masternode receiving address (into which you deposited your 1000 DASH) in the search bar
    6. Click on the "hash" of the 1000 DASH transaction
    7. Copy the value for that hash and paste it into your notes document
    8. Find the transaction listed under "outputs"
    9. Locate the index number of your unspent 1000 DASH (0 or 1), copy it into your notes

5. Create `masternode.conf`

  ```sh
  Local$ cd ~/Library/Application\ Support/Dash
  Local$ nano masternode.conf
  # this file may already exist
  # if it does not nano will create it
  # either way you will need to select a name/alias (e.g. "MNO1"
  # reminder: your masternode/VPS ip address can be found using 'nano ~/.ssh/config'
  # populate the file with the data obtained above in the following format:
  < # start of file contents
    MN01 <masternode-ip-address>:9999 <masternode-privkey> <txn-hash> <txn-output-index>
  > # end of file contents, exit with ctrl+x, save the file
  ```
  * You will need a separate line for each masternode in `masternode.conf`
  * You may add as many masternodes to the same wallet and `masternode.conf` as you wish
6. Close Dash-Qt

## 9. Start your masternode(s)
1. Launch Dash-Qt
  * This will now use the new settings from `dash.conf` and `masternode.conf`
2. Open a Dash-Qt console session (Tools > Debug console)
3. Enter the following to activate your remote masternode

    ```sh
    Dash$ walletpassphrase <your-wallet-passphrase> 60 # this is the same password you created to encrypt your wallet
    Dash$ masternode start-alias <alias-of-masternode-you-want-to-start>
    ```
  * You should get a response similar to:

    ```sh
    Dash$
    {
    "alias" : "MN01",
    "result" : "successful"
    }
    ```

4. Check your work by going back to your remote server and enter the following

    ```sh
    Local$ ssh <login-user>@<ip.add.re.ss>  
    VPS$ ./dash-cli masternode list full | grep <your MN IP address>
    ```
  * If it doesn't show enabled, don't panic; the blockchain must fully download on the remote server before it becomes active
  * You can check the status of the blockchain download by running the "getinfo" command repeatedly until it is fully caught up to the current number of blocks
  * Once it is caught up, give it a minute and try again to see if it shows as "ENABLED"
  * If your address comes up with "ENABLED" in the string, you've done everything correct.  Congratulations, your masternode is set up correctly, you are done!
