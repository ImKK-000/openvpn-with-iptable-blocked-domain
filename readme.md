# Note Command

```sh
# create vpn via docker
OVPN_DATA="ovpn-data"
docker volume create --name $OVPN_DATA
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_genconfig -u tcp://<hostname>:<port>
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn ovpn_initpki
docker run -v $OVPN_DATA:/etc/openvpn --name vpn-service -d -p <port>:1194/tcp --cap-add=NET_ADMIN kylemanna/openvpn
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
docker run -v $OVPN_DATA:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

# first, create chain rule
sudo iptables -N BLOCKED-DOMAIN
sudo iptables -I FORWARD -j BLOCKED-DOMAIN

# update domain pattern
sudo iptables -F BLOCKED-DOMAIN
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "adthor.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "aerserv.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appcoachs.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appier.net" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appnext.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "appsflyer.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "atdmt.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "baidu.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "clickhubs.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "cloudmobi.net" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "doubleverify.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "ecsdk.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "ggpht.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "inmobi.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "lenzmx.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "liftoff.io" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "llnwd.net" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "mobrand.net" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "mopub.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "sevmob.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "unlimapps.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "yeahmobi.com" -j DROP
sudo iptables -I BLOCKED-DOMAIN -m string --algo bm --string "zcoup.com" -j DROP
sudo iptables -L BLOCKED-DOMAIN

# if you want to store package to logging file (warning mode)
sudo iptables -A BLOCKED-DOMAIN -j LOG --log-prefix "IPTables-LOG: " --log-level 4
sudo tail -f /var/log/kern.log | grep --line-buffered "IPTables"
```
