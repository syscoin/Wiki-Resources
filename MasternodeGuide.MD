## Getting Started

To use this procedure you must have upgraded to the latest [Syscoin Qt 4.x](https://github.com/syscoin/syscoin/releases)  

If you are still on Syscoin 3.x then you will need to follow the [Upgrade Guide](https://github.com/syscoin/syscoin/wiki/1.-Upgrading-from-Syscoin-3.x-to-Syscoin-4.x)
#### _Click arrows to expand._
<details><summary>Syscoin 4.x Masternode Requirements</summary>

> 1. 100,000.00 SYSCOIN
> 
> 2.  VPS with following;
>  * 64-bit CPU?—?2 Cores (4 preferred)
>  * 4gb RAM (real) minimum (8gb RAM preferred)
>  * 4gb swap (if less than 8gb real RAM) Will need to use SSD if using Swap
>  * VM or OpenVZ (KVM preferred)
>  * Linux OS?—?Ubuntu 18.04.1 LTS (Bionic Beaver) preferred
>  * 80gb Disk Space (100gb+ SSD preferred  

</details>

<details><summary>List of VPS Providers</summary>

> 
> There are many VPS service providers that offer and exceed the hardware requirements, as such it is recommended that you shop around and do your own homework on various potential providers. Note the following is a list of just some examples and should not be interpreted as recommendations or endorsement.  
> 
> * [UK2.net](http://UK2.net)
> * [IONOS.co.uk](http://IONOS.co.uk)
> * [InterServer.net](http://InterServer.net)
> * [OVH.co.uk](http://OVH.co.uk)
> * [KimSufi.com](http://KimSufi.com)
> * [netcup.de](http://netcup.de)
> * [hetzner.de](http://hetzner.de)
> * [mVPS.net](http://mVPS.net)
> * [VPS-Mart.com](http://VPS-Mart.com)
> * [Hostinger.com](http://Hostinger.com)
> * [BudgetVM.com](http://BudgetVM.com)
> * [Virtono.com](http://Virtono.com)
> * [LeaseWeb.com](http://LeaseWeb.com)
> * [HomeAtCloud.com](http://HomeAtCloud.com)
> * [IdeaStack.com](http://IdeaStack.com)
> * [SimplyHosting.com](http://SimplyHosting.com)
> * [RAMNode.com](http://RAMNode.com)
> * [Time4VPS.com](http://Time4VPS.com)  
> 
</details>

## Installation
To use this procedure you must have upgraded to the latest [Syscoin Qt 4.x](https://github.com/syscoin/syscoin/releases)  

If you are still on Syscoin 3.x then you will need to follow the [Upgrade Guide](https://github.com/syscoin/syscoin/wiki/1.-Upgrading-from-Syscoin-3.x-to-Syscoin-4.x)

<details><summary>1. Getting Prepared</summary>

> To stake your masternode you will need to provide exactly 100,000 SYS in your masternode address. Use Syscoin Core Qt for your system to process this transaction.
> 
> Wait for your local Syscoin Core Qt to fully sync  

</details>

<details><summary>2. Unlocking Syscoin Qt</summary>



> To unlock your wallet: go to Window-> Console and type:  
> ```walletpassphrase "my pass phrase" 180```  
> _(Replace "my pass phrase" with yours. 180 is the amount of time to remain unlocked in seconds)_  
> and press and ENTER  

</details>

<details><summary>3. Generating a Masternode Key
</summary>  

> 
> To generate your masternode private key, go to Console and type:  
> ```masternode genkey```  
> and press ENTER 
> 
> * Copy this value as you will need it later, it will look similar to the following:
> 
> ![Console](https://cdn-images-1.medium.com/max/1600/1*Grml-2jIptuA8KwtLS51qg.png)
> 
</details>

<details><summary>4. Generating A New Legacy Address</summary>  

> 
> **Do not reuse Syscoin 3 addresses.** You will need to create a new HD address.
> Use the Console to generate your MN collateral legacy address. Do NOT press Receive to create it, as it will be the wrong type.
> 
> To generate a new legacy address, type:  
> `getnewaddress <LABEL> legacy`  
> *(replacing <LABEL> with a name, for example masternode1)*
> press ENTER  
> 
> Masternodes require legacy addresses, and will not work with the new Bech32 addresses that start with sys. Copy down this address as well as you will need to send your collateral to it in the next step.
> 
> If the address starts with "sys1" you have NOT included legacy and will need to regenerate a legacy address.
> If the address starts with "3" you have not used the console.
> 
> ![](https://cdn-images-1.medium.com/max/1600/1*jIwuvqKv7IXYiODkGx2Wsw.png)
>
> If you are configuring multiple masternodes you will need to create a unique masternode private key and unique collateral address for each masternode using the steps above. Once the address is created for each masternode send a collateral transaction of exactly 100,000SYS to the address for each masternode using the next steps.  
>
> **WARNING:** If you use the same address for multiple masternodes your reward payouts cannot be completed.
</details>

<details><summary>5. Sending 100,000 SYS Collateral</summary>

> Use Coin Control to ensure that you send your collateral from the correct address. Go to “Send” and then “Inputs” to select the input that you would like to send from.
> Please note this is a Test wallet and balances are not real
>
> Next enter your masternode collateral address from the previous step into the “Pay To” field. Enter “100,000” exactly into the “Amount” field and do NOT subtract fees from the amount as it will reduce your collateral total.
>
> Press “Send” to send your Syscoin to your masternode collateral address. You will need to wait 1 block?—?approximately 1 minute?—?for the transaction to confirm.
> Next you will need to get the transaction ID for the collateral by selecting the “Transactions” tab to see ‘payment to yourself’. Right-click this transaction to view the ID, and copy it for later use.
>
</details>

<details><summary>6. Getting your Output Index</summary>  

* In Syscoin Core Qt, open Console and type:  
`masternode outputs`  
and press ENTER  

You will get a long string that is a hash of the transaction id from the previous step followed by a number, usually “0” or “1” but can be between 1 and 999 to indicate the output index. The result should resemble the following:

</details>
Next we will have to edit the masternode.conf file.

For WIN
C:\Users\YOURUSERNAME\AppData\Roaming\Syscoin
Or type %appdata% in the search box and click on Syscoin folder
For MAC
 CMD + Shift + G
 ~/Library/Application Support/Syscoin
You will need to enter your masternode information using a text editor in the following format and use the public IP address of your server not your local computer. Make sure that the line does not start with a # as this will comment out the line!
masternode.conf
# Masternode config file
# Format: alias IP:port masternodeprivkey collateral_output_txid collateral_output_index
mn1 123.123.123.123:8369 7ra1rhngvNkhkiFE8STrmvH3LvYTCzLyRFHFsZvrJUBV6ZmWnc 06e38868bb8f9958e34d5155437d009b72dff33fc87fd42e51c0f74fdb 0

Save this file then close and restart Syscoin-Qt. If you don’t see your masternode listed in the “Masternode” tab please double check the above configuration.

If you now go to Coin Control you will see you collateral and it will have a padlock indicating it is locked. If you send any Syscoin from this wallet make sure that your collateral is locked.


CONFIGURE MASTERNODE ON VPS USING A SCRIPT

It’s a good idea to read through any script you find on the Internet before running it on a server with root privileges, including this one made by the community.
https://raw.githubusercontent.com/Syscoin/Masternode-install-script/master/script.sh
Finally we are ready to work on your server. Connect to your VPS as root via SSH (Putty) and enter the following command to start the automated install:
Note this has only been tested on Ubuntu 16.04 and 18.04
bash <(curl -sL https://raw.githubusercontent.com/Syscoin/Masternode-install-script/master/script.sh)
Default values are found in brackets and pressing enter will selected the [default] value. For entries with a [Y/n] the capital letter is the default. Enter [Y] to choose “yes” or [N] to choose “no”. Likely the only value you will need to enter is your masternode private key.
Syscoin Core Github Branch [master]: 
Masternode Private Key []: Your Masternode private key
External IP Address [123.123.123.123]: 
Masternode Port [8369]:
Configure for mainnet? [Y/n]: 
 
Press any key to continue or Ctrl+C to exit...
Once the build process and configuration have completed, to access the syscoind and syscoin-cli executables via the new syscoin user type the below into cmd;
source ~/.bashrc
Check you are fully Synced and geth_sync_status = synced
syscoin-cli getblockchaininfo
You will get this error until you start your Masternode as below

"status": "Not capable masternode: Masternode not in masternode list"
START MASTERNODE

Back on your local computer restart Syscoin Core Qt and wait for it to sync up to the network.
Choose the “Masternodes” tab, select your masternode,
Click “Initialize”.

Only click this button once and if your “Status” ever changes it’s recommended to confer with the #masternodes Discord Channel before restarting your node. If you restart you will need to re-qualify for rewards and won’t receive any rewards during this time.

Eligibility for Rewards

Keep in mind that your masternode will not immediately be eligible for rewards. The eligibility period is determined by the formula ([number of masternodes] * 4) /60 in Hours.

Note, if you restart your masternode by pressing “Initialize” in Qt this counter will reset and you will not receive rewards until your masternode is eligible again.

Summary

This script installs the necessary dependencies to build the Syscoin Core from source. It creates a user named “syscoin” and uses a systemd service to start the syscoind process as the “syscoin” user, and it set to start on boot after the necessary networking services have started.

Updates and reconfigurations can be performed by entering the command sysmasternode or the initial auto install command bash <(curl -sL https://raw.githubusercontent.com/Syscoin/Masternode-install-script/master/script.sh)

SYSHUB

Now it is time to head over to SysHub and register your masternode. You can also create Proposals through the generator for Governance and Vote on upcoming Proposals. As the Governance has increased significantly we urge you to take part in voting.

GOVERNANCE
AS A PROUD OWNER OF A SYSCOIN MASTERNODE YOU ARE ELIGIBLE TO TAKE PLACE IN GOVERNANCE VOTING PLEASE TAKE THE TIME ONCE A MONTH TO VOTE SEE SYSHUB FOR DETAILS.
THE AMOUNT OF GOVERNANCE HAS INCREASED SIGNIFICANTLY SO MAKE YOUR VOTE COUNT.
MASTERNODE COMMANDS IF YOU USED THE SCRIPT
View your syscoin.conf

nano /home/syscoin/.syscoin/syscoin.conf
View your sentinel.conf

nano /home/syscoin/sentinel/sentinel.conf
View the syscoin user crontab which should contain:

sudo crontab -u syscoin -l
Run a sentinel ping to speed up Qt syncing? why not!

sudo su -c "sentinel-ping" syscoin
View the sentinel-ping cron log, look for errors

sudo less /home/syscoin/sentinel/sentinel-cron.log
View the syscoind debug log, look for errors

sudo less /home/syscoin/.syscoin/debug.log
Start and stop the syscoind systemd service

sudo service syscoind stop
sudo service syscoind start
sudo service syscoind restart
Check that the syscoind process is running at the proper user

ps aux | grep [s]yscoind
Log out and back in or run the following to alias syscoind and syscoin-cli

source ~/.bashrc
now the commands run as the syscoin user

syscoin-cli getblockchaininfo
syscoin-cli mnsync status
syscoin-cli masternode status
It is aliased to this shorter function

syscli getblockchaininfo
syscli mnsync status
syscli masternode status
If you really want to log in as the syscoin user

sudo su - syscoin
Special thanks to demesm and doublesharp for the initial script, Bigpoppa for most of the conversion and bitje, johnp and the Syscoin team for upgrading and working out minor issues to get it running on SYS4.
MASTERNODE HELP
Never Accept Help via a Direct Message there are many scammers about and never give ANY files to anyone, the Syscoin Team will never ask for them.
SysHub: Register your masternodes with Syshub. You can also create Proposals through the generator for Governance and Vote on upcoming Proposals.

Syscoin Discord: If you require more help, jump into the Syscoin Discord and our community will be more than happy to help you out!

Sysnode.info: This website has an array of tools such as Masternode Stats, Monitoring and keeping up to date with current news with Syscoin.

CONFIGURE MASTERNODE ON VPS MANUALLY
You will have needed to have set your QT up as above then use this guide.

https://medium.com/@bittyjohn1954/syscoin-4-masternodes-manual-setup-373e55bba6eb
PLEASE ENSURE ONCE YOU HAVE FINISHED SETTING UP YOUR MASTERNODES THAT YOU MAKE BACKUPS OF YOUR WALLET.DAT