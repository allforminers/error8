
```
###u18

sudo apt -y install git

git clone https://github.com/allforminers/error8.git

cd error8

sudo apt-get install -y unzip

unzip yiimp_install_scrypt-master.zip

cd yiimp_install_scrypt-master

bash install.sh


###Org.Link

git clone https://github.com/eskal/yiimp_install_scrypt.git
cd yiimp_install_scrypt/
bash install.sh


###after DONE !!!

edit /var/web/yaamp/modules/site
files edit
SiteController.php line 11

cd /var/web/yaamp/modules/site
nano SiteController.php

public function actionmyadmin()
to
public function actionmypanel()

Edit .conf Stratum Mining DIFF

sha. edit

[STRATUM]
algo = sha256
difficulty = 750000
nicehash = 1000000
max_ttf = 40000
reconnect = 1

cd /var/stratum
screen bash run.sh sha

kill proces
screen -X -S 31996.pts-11.vmi439482 kill


###Next STEP


### ERROR: RPC Error: error -8: label argument must be a valid label name or "*"

edit coin_results.php,
/var/web/yaamp/modules/site/coin_results.php line 306 change ticker 

example 

$account = '';
if ($DCR || $DGB ) $account = '*';
else if ($ETH) $account = $coin->master_wallet;
if ($coin->symbol=="TESTB" || $coin->symbol=="LTC") $account = "*";

cd /var/web/yaamp/modules/site
nano coin_results.php 
line 306

--------------
--------------






OTHER STEP

 CHANGES:
--------------
PAYMENTS missing FIX:
edit /var/web/yaamp/core/backend/payment.php and on line 57 you will need to add: 
$coin->symbol == 'XXX'

SECURITY 'INJECTION' FIX:
edit: yaamp/modules/site/wallet.php (line ~26)
if (!is_string($address)) {
        throw new Exception("Do not try to hack !!");
}
if (mb_strlen($address) > 0 && !ctype_alnum($address)) {
        throw new Exception("You are a bad boy !!");
}

GETBLOCKTEMPLATE FIX:
edit coin_results.php on line 300
$account = '';
if ($DCR) $account = '';
if ($ETH) $account = $coin->master_wallet;
if ($coin->symbol=="XXX") $account = "";// make sure to change XXX to coin symbol 

ERROR: RPC Error: error -8: label argument must be a valid label name or "*" FIX:
in /web/yaamp/backend/coins.php at line 130 add 
if($coin->symbol == 'XXX')
            $template = $remote->getblocktemplate('{"rules":["segwit"]}');
            else
 should be just above 
$template = $remote->getblocktemplate('{}');

WALLET-RPC.php
==============
depending on your coin/core you may need to edit /var/web/yaamp/core/rpc/wallet-rpc.php -- enable some debug checkpoints

STRATUM CHANGES:
----------------
You need to edit and MAKE and overwrite the original binaries
/stratum/coind.cpp


If you are on gensis 0 block change code:
If (!coind->height)
// change to
if (coind->height<0)
(dont forget to change it back after block 1 is found)

line 122 "bool getaddressinfo = ((strcmp(coind->symbol,"DGB") == 0)  (strcmp(coind->symbol2, "DGB") == 0)  (strcmp(coind->symbol, "XXX") == 0));"
XXX = your coin

MAKE

.CONF CHANGES:
--------------
change the blocknotify=xxx to /var/stratum/blocknotify

---------------------------------------------------------------------
---------------------------------------------------------------------
Add Dedicated Port for each coin:
1. Coin to be added: Reden (REDEN) algorithm x16s and port for this coin will be 12345
2. Create a file called reden.conf under /var/stratum/config which is copy of x16s.conf
3. Open a file reden.conf
4. Chagne the port to 12345
5. Add below lines in the end of the file:
    [WALLETS]
    include = REDEN
6. Save the file reden.conf
7. Open the file called x16s.conf
8. Add below lines in the end of the file:
    [WALLETS]
    exclude = REDEN
9. Save the file x16s.conf
10. Start the stratum for reden.conf using below command
    sudo screen -S reden
    sudo /var/stratum/run.sh reden
11. go to folder ~/.redencore/
12. stop the daemon for coin
13. open the coin configuration file (reden.conf)
14. Change the port in the blocknotify line.
15. Save the file reden.conf
16. Start the coin daemon.
17. Check the file /var/stratum/config/reden.log it should show that stratum is trying to connect to the coin
18. Open the port using sudo ufw allow 12345/tcp 
19. Start mining to dedicated port
 
----exampel of reden.conf in stratum/config/----
 
////////////////////////////////////////////////////
[TCP]
server = swedpool.eu
port = 12345
password = hCMkqCLsnBRYheIrH5Asysl
 
[SQL]
host = localhost
database = yiimpfrontend
username = stratum
password = whiGSN2cn3xcJJ
 
[STRATUM]
algo = x16s
difficulty = 0.25
max_ttf = 50000
 
[WALLETS]
    include = REDEN
///////////////////////////////////////////////////////
 
----exampel of x16s.conf in stratum/config/----
 
//////////////////////////////////////////////////////
 
[TCP]
server = swedpool.eu
port = 3663
password = nBRYheIrH5Asysl
 
[SQL]
host = localhost
database = yiimpfrontend
username = stratum
password = 4HYAwhiGSN2cn3xcJJ
 
[STRATUM]
algo = x16s
difficulty = 0.25
max_ttf = 50000
 
[WALLETS]
    exclude = REDEN

```
