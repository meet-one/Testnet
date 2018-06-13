### How to install cleos to voting block producer 

#### BM推荐的投票工具"cleos"，本教程cleos安装方法来自于eosio官方教程 
#### [https://developers.eos.io/eosio-nodeos/docs/docker-quickstart](https://developers.eos.io/eosio-nodeos/docs/docker-quickstart)

#### 1. Install docker / 安装Docker

#### [https://docs.docker.com/install](https://docs.docker.com/install/)

You have to regiter an account before download docker / 需要用邮箱注册一个账号才可以下载

#### 2. Download eosio image from docker / 在命令行执行以下命令，下载EOS官方源代码镜像
```
docker pull eosio/eos-dev
```

#### 3. Initial local node / 启动本地节点以及cleos工具
```
sudo docker run --rm --name eosio -d -p 8888:8888 -p 9876:9876 -v /tmp/work:/work -v /tmp/eosio/data:/mnt/dev/data -v /tmp/eosio/config:/mnt/dev/config eosio/eos-dev  /bin/bash -c "nodeos -e -p eosio --plugin eosio::wallet_api_plugin --plugin eosio::wallet_plugin --plugin eosio::producer_plugin --plugin eosio::history_plugin --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin --plugin eosio::http_plugin -d /mnt/dev/data --config-dir /mnt/dev/config --http-server-address=0.0.0.0:8888 --access-control-allow-origin=* --contracts-console"
```

#### 3.5 添加cleos快捷方式
```
alias cleos='docker exec eosio /opt/eosio/bin/cleos --wallet-url http://localhost:8888'
```

#### 4. Teset cleos / 测试cleos工具是否正常
```
cleos -u https://mainnet.meet.one system listproducers
```

#### 5. Create Wallet / 创建钱包
```
cleos wallet create 
# save your password / 保存好生成的钱包解锁密码
```

#### 6. Query your account name / 查询账户名
```
# [[publick_key]] = your publick key / [[publick_key]]是你的公钥地址
cleos -u https://mainet.meet.one get accounts [[publick_key]]
# save your account name / 保存好你的账户名
```

#### 7. Import private key / 导入私钥
```
# [[private_key]] = your private key / [[private_key]]是你的私钥
cleos wallet import [[private_key]] 
```

#### 8. Vote for block producers / 给超级节点投票，多个节点用空格隔开
```
# [[account_name]] = your eos account name / 把[[account_name]]替换成你的EOS账户名
cleos -u https://mainnet.meet.one system voteproducer prods [[account_name]] eosiomeetone eosiosg11111 eoscannonchn
```

#### 9. Approve for new block producers  / 投完票以后，如果想要继续给某个BP投票
```
# [[account_name]] = your eos account name / 把[[account_name]]替换成你的EOS账户名
cleos -u https://mainnet.meet.one system voteproducer approve [[account_name]] eosiomeetone
```

#### 10. Stop docker / 停止Docker
```
docker stop eosio
```

