# eos-script

Shell scripts to install and launch an EOSIO node.

## 1.Install

#### Install EOSIO
```
cd ~
git clone https://github.com/EOS-Mainnet/eos.git
cd eos
git checkout tags/mainnet-1.1.1
git submodule update --init --recursive
# modify eosio_build.sh, change CORE_SYMBOL_NAME="SYS" to CORE_SYMBOL_NAME="EOS"
./eosio_build.sh
cd build
sudo make install
```


#### Verify EOSIO installation
```
export PATH=${HOME}/opt/mongodb/bin:$PATH
/root/opt/mongodb/bin/mongod -f /root/opt/mongodb/mongod.conf &
cd /root/eos/build; make test
```


#### Add the eosio bin folder to your PATH
```
echo 'PATH=$PATH:/usr/local/eosio/bin/' >> ~/.profile
```

#### Download config-dir folder to your home directory


## 2.Before Launch

#### Create your wallet
```
cleos wallet create  # save your password
```


#### Create your account
```
cleos create key # save your public/private key
```


#### Import your account 
```
cleos wallet import --private-key _PRIVATE_KEY_
```


## 3.Launch

#### Launch node
```
cd ~
mkdir logs
nohup nodeos --data-dir ./data-dir --config-dir ./config-dir > ./logs/nodeos.log 2>&1 &
```

## 4.Deploy Contract

#### Deploy Token Contract
```
# create new account to deploy contract
cleos create account eosio eosio.msig EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.names EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.ram EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.ramfee EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.saving EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.stake EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.token EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
cleos create account eosio eosio.upay EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
```
    
#### deploy token contract
```
cleos set contract eosio.token $EOS_BUILD_DIR/contracts/eosio.token -p eosio.token
cleos set contract eosio.msig $EOS_BUILD_DIR/contracts/eosio.msig -p eosio.msig
```

#### Create Token
```
# create token
cleos push action eosio.token create '[ "eosio", "1000000000.0000 EOS", 0, 0, 0]' -p eosio.token 
# Issue token
cleos push action eosio.token issue '[ "eosio", "1000000000.0000 EOS", "memo" ]' -p eosio

```

#### Deploy System Contract
```
cleos set contract eosio $EOS_BUILD_DIR/contracts/eosio.system -p eosio
```

#### create new accounts
```
cleos system newaccount eosio meetonetest1 EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S --stake-net "1000.0000 EOS" --stake-cpu "1000.0000 EOS" --buy-ram "1000.0000 EOS"
cleos system newaccount eosio meetonetest2 EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S --stake-net "1000.0000 EOS" --stake-cpu "1000.0000 EOS" --buy-ram "1000.0000 EOS"
# issue token to test account
cleos push action eosio.token transfer '[ "meetonetest1", "500000000.0000 EOS", "memo" ]' -p eosio
cleos push action eosio.token transfer '[ "meetonetest2", "500000000.0000 EOS", "memo" ]' -p eosio
```

#### Test Transfer
```
cleos push action eosio.token transfer '[ "meetonetest1", "meetonetest2", "100.0000 EOS", "test transfer" ]' -p meetonetest1
cleos get currency balance eosio.token meetonetest1
cleos get currency balance eosio.token meetonetest2
```

## 5.Register Producer

#### Create BP account
```
cleos system newaccount eosio meetonetest3 EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S --stake-net "1000.0000 EOS" --stake-cpu "1000.0000 EOS" --buy-ram "1000.0000 EOS"
```


#### Register Producer
```
cleos system regproducer meetonetest3 EOS6vX273GvFCRfHjt8PQKyM9ecocUQnqkS9FGVb5Uid3xjGzCR7S https://meet.one
```

## 6. Vote

```
cleos system delegatebw meetonetest1 meetonetest1 "1000.0000 EOS" "1000.0000 EOS" --transfer
cleos system voteproducer prods meetonetest1 meetontest3
```

## 7. Check

#### Check account status
```
cleos get table eosio eosio userres
```

#### List Producers
```
cleos system listproducers
```
