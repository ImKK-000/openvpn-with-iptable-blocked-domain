# Note Command

```sh
# create vpn via docker
OVPN_DATA="ovpn-data"
docker volume create --name $OVPN_DATA
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_genconfig -u tcp://<hostname>:<port>
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn ovpn_initpki
docker run -v $OVPN_DATA:/etc/openvpn --name vpn-service -d -p 443:1194/tcp --cap-add=NET_ADMIN kylemanna/openvpn
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

# first, create chain rule
sudo iptables -N BLOCKED-DOMAIN
sudo iptables -I FORWARD -j BLOCKED-DOMAIN

# update domain pattern
sudo iptables -F BLOCKED-DOMAIN
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appnext.com" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "zcoup.com" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "cloudmobi.net" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appsflyer.com" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appier.net" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "llnwd.net" -j REJECT
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "mopub.com" -j REJECT
sudo iptables -L BLOCKED-DOMAIN
```
