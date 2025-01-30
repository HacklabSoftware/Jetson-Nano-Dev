nmcli con show 

sudo nmcli con mod "eth0" \
  ipv4.addresses "192.168.1.70/24" \
  ipv4.gateway "192.168.1.1" \
  ipv4.dns "8.8.8.8 1.1.1.1" \
  ipv4.dns-search "8.8.4.4" \
  ipv4.method "manual"

[https://stackoverflow.com/questions/66384210/how-te-set-a-static-ip-for-a-jetson-nano]
