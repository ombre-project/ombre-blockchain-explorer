
# ombre Blockchain Explorer

Forked from [Onion Monero Blockchain Explorer](https://github.com/moneroexamples/onion-monero-blockchain-explorer)

Full featured ombre blockchain explorer for privacy-oriented users:

 - Does not use JavaScript,
 - Does not have images which might be used for [cookieless tracking](http://lucb1e.com/rp/cookielesscookies/),
 - Does not track users activates through google analytics,
 - Open source,
 - Supports ombre testnet nor stagenet networks,
 - Full featured JSON API.


Libraries:

 - [crow](https://github.com/ipkn/crow) - C++ micro web framework
 - [mstch](https://github.com/no1msd/mstch) - C++ {{mustache}} templates
 - [json](https://github.com/nlohmann/json) - JSON for Modern C++
 - [fmt](https://github.com/fmtlib/fmt) - Small, safe and fast string formatting library

## Explorer hosts

 - [TBA) - Official Explorer

## ombre Blockchain Explorer features

The key features of the ombre Blockchain Explorer are:

 - no cookies, no web analytics trackers, no images,
 - by default no JavaScript, but can be enabled for client side decoding and proving transactions,
 - open sourced,
 - made fully in C++,
 - showing encrypted payments ID,
 - showing ring signatures,
 - showing transaction extra field,
 - showing public components of ombre addresses,
 - decoding which outputs and mixins belong to the given ombre address and viewkey,
 - can prove that you send ombre to someone,
 - detailed information about ring members, such as, their age, timescale and their ring sizes,
 - showing number of amount output indices,
 - support ombre testnet and stagnet networks,
 - tx checker and pusher for online pushing of transactions,
 - estimate possible spendings based on address and viewkey,
 - can provide total amount of all miner fees,
 - decoding encrypted payment id,
 - decoding outputs and proving txs sent to sub-address.


## Compilation on Ubuntu 16.04/18.04

##### Compile latest ombre development version

Download and compile recent ombre into your home folder:

```bash
# first install ombre dependecines
sudo apt update

sudo apt install build-essential cmake pkg-config libboost-all-dev libssl-dev libzmq3-dev libunbound-dev libsodium-dev libminiupnpc-dev libunwind8-dev liblzma-dev libreadline6-dev libldns-dev libexpat1-dev doxygen graphviz libpcsclite-dev

# go to home folder
cd ~

git clone --recursive https://github.com/ombre-project/ombre

cd ombre/

make
```

##### Compile and run the explorer

Once the ombre is compiled, the explorer can be downloaded and compiled
as follows:

```bash
# go to home folder if still in ~/ombre
cd ~

# download the source code
git clone https://github.com/ombre-project/ombre-blockchain-explorer

# enter the downloaded sourced code folder
cd ombre-blockchain-explorer

# make a build folder and enter it
mkdir build && cd build

# create the makefile
cmake -DMONERO_DIR=~/ombre ..

# also can build with ASAN (sanitizers), for example
cmake -DSANITIZE_ADDRESS=On ..

# compile
make
```


To run it:
```
./xmrblocks -b ~/.ombre/lmdb/ --enable-pusher 1 --enable-mixin-details 1 --enable-key-image-checker 1 --enable-output-key-checker 1 --enable-json-api 1 --enable-tx-cache 1 --show-cache-times 1 --enable-block-cache 1 --enable-js 1 --enable-autorefresh-option 1 --enable-emission-monitor 1 --no-blocks-on-index 50 -d 127.0.0.1:19744 
```

Example output:

```bash
user@arch:~/ombre-explorer/build$ ./xmrblocks -b /home/user/.ombre/lmdb
Staring in non-ssl mode
(2018-07-27 01:06:02) [INFO    ] Crow/0.1 server is running at 127.0.0.1:8081 using 4 threads
```

Go to your browser: http://127.0.0.1:8081

## The explorer's command line options

```
xmrblocks, ombre Blockchain Explorer:
  -h [ --help ] [=arg(=1)] (=0)         produce help message
  -t [ --testnet ] [=arg(=1)] (=0)      use testnet blockchain
  -s [ --stagenet ] [=arg(=1)] (=0)     use stagenet blockchain
  --enable-pusher [=arg(=1)] (=0)       enable signed transaction pusher
  --enable-mixin-details [=arg(=1)] (=0)
                                        enable mixin details for key images,
                                        e.g., timescale, mixin of mixins, in tx
                                        context
  --enable-key-image-checker [=arg(=1)] (=0)
                                        enable key images file checker
  --enable-output-key-checker [=arg(=1)] (=0)
                                        enable outputs key file checker
  --enable-json-api [=arg(=1)] (=1)     enable JSON REST api
  --enable-tx-cache [=arg(=1)] (=0)     enable caching of transaction details
  --show-cache-times [=arg(=1)] (=0)    show times of getting data from cache
                                        vs no cache
  --enable-block-cache [=arg(=1)] (=0)  enable caching of block details
  --enable-js [=arg(=1)] (=0)           enable checking outputs and proving txs
                                        using JavaScript on client side
  --enable-autorefresh-option [=arg(=1)] (=0)
                                        enable users to have the index page on
                                        autorefresh
  --enable-emission-monitor [=arg(=1)] (=0)
                                        enable ombre total emission monitoring
                                        thread
  -p [ --port ] arg (=8081)             default explorer port
  --testnet-url arg                     you can specify testnet url, if you run
                                        it on mainnet or stagenet. link will
                                        show on front page to testnet explorer
  --stagenet-url arg                    you can specify stagenet url, if you
                                        run it on mainnet or testnet. link will
                                        show on front page to stagenet explorer
  --mainnet-url arg                     you can specify mainnet url, if you run
                                        it on testnet or stagenet. link will
                                        show on front page to mainnet explorer
  --no-blocks-on-index arg (=10)        number of last blocks to be shown on
                                        index page
  --mempool-info-timeout arg (=5000)    maximum time, in milliseconds, to wait
                                        for mempool data for the front page
  --mempool-refresh-time arg (=5)       time, in seconds, for each refresh of
                                        mempool state
  -b [ --bc-path ] arg                  path to lmdb folder of the blockchain,
                                        e.g., ~/.ombre/lmdb02
  --ssl-crt-file arg                    path to crt file for ssl (https)
                                        functionality
  --ssl-key-file arg                    path to key file for ssl (https)
                                        functionality
  -d [ --deamon-url ] arg (=http:://127.0.0.1:12211)
                                        ombre deamon url
```

## Enable ombre emission

Obtaining current ombre emission amount is not straight forward. Thus, by default it is
disabled. To enable it use `--enable-emission-monitor` flag, e.g.,


```bash
xmrblocks --enable-emission-monitor
```

This flag will enable emission monitoring thread. When started, the thread
 will initially scan the entire blockchain, and calculate the cumulative emission based on each block.
Since it is a separate thread, the explorer will work as usual during this time.
Every 10000 blocks, the thread will save current emission in a file, by default,
 in `~/.ombre/lmdb/emission_amount.txt`. For testnet or stagenet networks,
 it is `~/.ombre/testnet/lmdb/emission_amount.txt` or `~/.ombre/stagenet/lmdb/emission_amount.txt`. This file is used so that we don't
 need to rescan entire blockchain whenever the explorer is restarted. When the
 explorer restarts, the thread will first check if `~/.ombre/lmdb/emission_amount.txt`
 is present, read its values, and continue from there if possible. Subsequently, only the initial
 use of the tread is time consuming. Once the thread scans the entire blockchain, it updates
 the emission amount using new blocks as they come. Since the explorer writes this file, there can
 be only one instance of it running for mainnet, testnet and stagenet. Thus, for example, you cant have
 two explorers for mainnet
 running at the same time, as they will be trying to write and read the same file at the same time,
 leading to unexpected results. Off course having one instance for mainnet and one instance for testnet
 is fine, as they write to different files.

 When the emission monitor is enabled, information about current emission of coinbase and fees is
 displayed on the front page, e.g., :

```
```

The values given, can be checked using ombre daemon's  `print_coinbase_tx_sum` command.
For example, for the above example: `print_coinbase_tx_sum 0 1313449`.

To disable the monitor, simply restart the explorer without `--enable-emission-monitor` flag.

## Enable JavaScript for decoding proving transactions

By default, decoding and proving tx's outputs are done on the server side. To do this on the client side
(private view and tx keys are not send to the server) JavaScript-based decoding can be enabled:

```
xmrblocks --enable-js
```

## Enable SSL (https)

By default, the explorer does not use ssl. But it has such a functionality.

As an example, you can generate your own ssl certificates as follows:

```bash
cd /tmp # example folder
openssl genrsa -out server.key 1024
openssl req -new -key server.key -out server.csr
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```

Having the `crt` and `key` files, run `xmrblocks` in the following way:

```bash
./xmrblocks --ssl-crt-file=/tmp/server.crt --ssl-key-file=/tmp/server.key
```

Note: Because we generated our own certificate, modern browsers will complain
about it as they can't verify the signatures against any third party. So probably
for any practical use need to have properly issued ssl certificates. Alternatively,
use an SSL terminator such as nginx.

## JSON API

The explorer has JSON api. For the API, it uses conventions defined by [JSend](https://labs.omniti.com/labs/jsend).
By default the api is disabled. To enable it, use `--enable-json-api` flag, e.g.,

```
./xmrblocks --enable-json-api
```

