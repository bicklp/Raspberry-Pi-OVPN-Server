# Raspberry-Pi-OVPN-Server
Setup Raspberry Pi as an openVPN server

## Update the Server
```
sudo -s
apt-get update
apt-get upgrade
```

## Install the software
```
apt-get install openvpn easy-rsa
mkdir /etc/openvpn/easy-rsa
cp /usr/share/easy-rsa /etc/openvpn/easy-rsa
```

## edit the vars file and change export EASY_RSA to export EASY_RSA="/etc/openvpn/easy-rsa"
```
cd /etc/openvpn/easy-rsa
nano vars
```

```
source ./vars
./clean-all
./build-ca
```
## build key for your server, name your server here
```
./build-key-server [server name]
** when prompted common name must equal [server name] **
** challenge password must be left blank **
```


## build key for your server, name your server here
```
./build-key-pass [vpn_username]
>challenge password must be left blank**
```


```
cd /etc/openvpn/easy-rsa/keys
openssl rss -in [vpn_username].key -des3 -out [vpn_username].3des.key
cd /etc/openvpn/easy/rsa
./build-dh
openvpn --genkey --secret keys/ta.key
```
## get server conf file and update for your local settings
```
wget https://github.com/bicklp/Raspberry-Pi-OVPN-Server/blob/master/server.conf -P /etc/openvpn/

```
## enable ipv4 forwarding uncomment net.ipv4.ip_forward=1
```
nano /etc/sysctl.conf
sysctl -p
```
## Get firewall rules and update to your local settings
```
cd /etc
wget https://github.com/bicklp/Raspberry-Pi-OVPN-Server/blob/master/firewall-openvpn-rules.sh
```


## Update your interface file and add in the firewall rules file from above
```
nano /etc/network/interfaces
>pre-up /etc/firewall-openvpn-rules.sh
**add line to interfaces file with a tab at the beginning**
```




```
reboot
```

#Client Setup


```
cd /etc/openvpn/easy-rsa/keys
```

```
wget https://github.com/bicklp/Raspberry-Pi-OVPN-Server/blob/master/Default.txt -P /etc/openvpn/easy-rsa/keys
```

```
nano Default.txt
#Set the Public IP or DDNS name in the Default.txt file
```

```
wget https://github.com/bicklp/Raspberry-Pi-OVPN-Server/blob/master/makeOVPN.sh -P /etc/openvpn/easy-rsa/keys
```

```
chmod 700 makeOVPN.sh
```

```
./makeOVPN.sh
#enter [vpn_username] when prompted
#export the [vpn_username].ovpn file to clients
```



