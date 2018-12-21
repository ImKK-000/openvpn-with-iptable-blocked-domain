# Note Command

```sh
OVPN_DATA="ovpn-data"
docker volume create --name $OVPN_DATA
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_genconfig -u tcp://192.168.1.100:443
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn ovpn_initpki
docker run -v $OVPN_DATA:/etc/openvpn --name vpn-service -d -p 443:1194/tcp --cap-add=NET_ADMIN kylemanna/openvpn
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

iptables -N BLOCKED-DOMAIN
iptables -I FORWARD -j BLOCKED-DOMAIN
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appnext.com" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "zcoup.com" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "cloudmobi.net" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appsflyer.com" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appier.net" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "llnwd.net" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "mopub.com" -j REJECT
iptables -I BLOCKED-DOMAIN -m string --algo bm --string "liftoff.io" -j REJECT
```