#### These are test only keys and should never be used for the production blockchain. 

Private key: 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3

Public key: EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV


### unlock wallet & import private key

```
cleos wallet unlock
```

```
cleos wallet import
```

### launch nodoes

```
cd local_testnet
sh ./start.sh
```

### issue EOS token

```
sh ./create_system_token.sh
```

### clean local testnet

```
sh ./clean.sh
```