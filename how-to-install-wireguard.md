
Install WireGuard
```
sudo add-apt-repository ppa:wireguard/wireguard
sudo apt-get update
sudo apt-get install wireguard
```

Generate Private Key
```
wg genkey > private
cat private
wg pubkey < private
```

Edit wg0.conf
```
[Interface]
Address =  192.168.0.2/24
PrivateKey = ****
ListenPort = ****

[Peer]
PublicKey = [PublicKey]
Endpoint = [InternetIP:Port]
AllowedIPs = 192.168.0.1/32
```

Set wg0.conf
```
sudo wg-quick up eos
# sudo wg-quick down eos
```
