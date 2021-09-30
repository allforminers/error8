

### ERROR: RPC Error: error -8: label argument must be a valid label name or "*" FIX:

```edit coin_results.php,
/var/web/yaamp/modules/site/coin_results.php line 306 change ticker 

example 

$account = '';
if ($DCR || $DGB ) $account = '*';
else if ($ETH) $account = $coin->master_wallet;
if ($coin->symbol=="TESTB" || $coin->symbol=="LTC") $account = "*";

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


```
