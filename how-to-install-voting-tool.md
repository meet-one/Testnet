# vote-tool-install

The tutorial of voting EOS block producer.

通过安装EOS源码进行投票的教程

## 1.Install

#### Install EOSIO / 安装EOSIO源代码
```
cd ~
git clone https://github.com/EOS-Mainnet/eos.git
cd eos
git checkout tags/mainnet-1.0.2.2
git submodule update --recursive
./eosio_build.sh
cd build
sudo make install
```

#### Create Symbolic Links / 创建cleos快捷方式
```
sudo ln -s ~/eos/build/programs/cleos/cleos /usr/local/bin/cleos
```

#### Create Wallet /创建钱包
```
cleos wallet create 
# save your password / 保存好生成的钱包解锁密码
```

#### Unlock your wallet / 解锁钱包
```
cleos wallet unlock
# input your wallet password / 输入前面生成的钱包密码
```

#### Import private key / 导入私钥
```
# [[private_key]] = your private key / [[private_key]]是你的私钥
cleos wallet import [[private_key]] 
```

#### Vote for block producers / 给超级节点投票，多个节点用空格隔开
```
# [[account_name]] = your eos account name / 把[[account_name]]替换成你的EOS账户名
cleos -u https://mainnet.meet.one system voteproducer prods [[account_name]] eosiomeetone eosiosg11111 eoscannonchn
```

#### Approve for new block producers  / 投完票以后，如果想要继续给某个BP投票
```
# [[account_name]] = your eos account name / 把[[account_name]]替换成你的EOS账户名
cleos -u https://mainnet.meet.one system voteproducer approve [[account_name]] eosiomeetone
```

