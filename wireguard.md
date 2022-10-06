# Wireguard

https://www.wireguard.com/

WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN.

## Network design

client -----> underlay -----> wireguard -----> LAN
0---------------------------------0

1. Install wireguard 

```
apt install wireguard
```

2. Allow routing on Linux hosts

```
sysctl -w net.ipv4.ip_forward=1
```

3. Allow the wireguard incoming traffic

```
# iptables -I INPUT -i wg0 -j ACCEPT
# iptables -I FORWARD -i ens18 -o wg0 -j ACCEPT
# iptables -I FORWARD -i wg0 -i ens18 -j ACCEPT
# iptables -I FORWARD -i ens18 -p udp --dport 51888 -j ACCEPT
```

4. Generate keys

```
wg genkey | sudo tee /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

## Client side configuration

```
# cat ./wg0.conf
[Interface]
PrivateKey = aD+o0nXXXXXXXXXXXXXXXXXXXG7CAGJ/TDFOONwfFM=
Address = 192.168.200.2/24

[Peer]
PublicKey = DVcuhbiXXXXXXXXXXXXXXXXXXXXXkeJ9qr5KcCpXkiM=
## Encryption domain ##
### Full tunnel ###
Allowed IPs = 0.0.0.0/0
### Split tunnel ###
# Allowed IPs = 192.168.0.0/22
Endpoint = 192.0.2.150:51888
```

## Server side configuration
```
# cat ./wg0.conf
[Interface]
## Private IP address for the wg0 interface ##
Address = 192.168.200.1/24

## VPN server listening port ##
ListenPort = 51888

## VPN server private key ##
PrivateKey = QA2d1MXXXXXXXXXXXXXXXXXXXXO/QlQwMhg7NIyqAWI=

## Masquerade, hide-nat rules ##
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens18 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens18 -j MASQUERADE

[Peer]
Publickey = Kpg4e7KoXXXXXXXXXXXXXXXXXXXXXd03cy8kKcXo7+1U=
## Encryption domain ##
AllowedIPs = 192.168.200.0/24
```

## Flap the tunnel

```
# wg-quick down wg0; wg-quick up wg0
```

