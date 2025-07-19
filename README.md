1. Clean install Rpi OS
2. `git clone https://github.com/Dalton8804/rpi-broadcast-wifi.git`
3. `chmod +x ./broadcast-wifi.sh`
4. `./broadcast-wifi.sh`


#### Steps
```
sudo apt update
sudo apt upgrade -y
sudo apt DEBIAN_FRONTENT=noninteractive install iptables iptables-persistent netfilter-persistent -y
sudo apt install isc-dhcp-server
sudo raspi-config nonint do_wifi_country US
sudo nmcli con add type ethernet ifname eth0 con-name eth0-shared autoconnect yes ip4 192.168.7.1/24
sudo sed -r --in-place 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g;' /etc/sysctl.conf
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
sudo netfilter-persistent save
echo -e "subnet 192.168.7.0 netmask 255.255.255.0 {\n\trange 192.168.7.10 192.168.7.50;\n\toption routers 192.168.7.1;\n\toption domain-name-servers 8.8.8.8, 1.1.1.1;\n}" | sudo tee -a /etc/dhcp/dhcpd.conf
sudo sed -r --in-place 's/INTERFACESv4=""/INTERFACESv4="eth0"/g;' /etc/default/isc-dhcp-server
sudo systemctl restart isc-dhcp-server
```


