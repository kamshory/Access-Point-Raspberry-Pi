# Setup Access Point with DHCP on Raspberry Pi


```bash
yum update -y
yum install -y dhcp
yum install -y nano

echo -e 'ESSID=PicoEdu' > /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'MODE=Ap' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'KEY_MGMT=WPA-PSK' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'MAC_ADDRESS_RANDOMIZATION=default' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'TYPE=Wireless' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'PROXY_METHOD=none' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'BROWSER_ONLY=no' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'BOOTPROTO=none' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPADDR=192.168.0.11' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'PREFIX=24' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'GATEWAY=192.168.0.1' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'DNS1=8.8.8.8' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'DEFROUTE=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV4_FAILURE_FATAL=no' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV6INIT=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV6_AUTOCONF=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV6_DEFROUTE=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV6_FAILURE_FATAL=no' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'IPV6_ADDR_GEN_MODE=stable-privacy' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'NAME=wlan0' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'UUID=605a8783-c38b-4351-8f28-e82f99fdd0c6' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo -e 'ONBOOT=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0

echo -e 'WPA_PSK=planetbiru' > /etc/sysconfig/network-scripts/keys-wlan0

echo '' > /etc/dhcp/dhcpd.conf
echo 'default-lease-time 600;' >> /etc/dhcp/dhcpd.conf
echo 'max-lease-time 7200;' >> /etc/dhcp/dhcpd.conf
echo 'authoritative;' >> /etc/dhcp/dhcpd.conf
echo '' >> /etc/dhcp/dhcpd.conf
echo 'subnet 192.168.0.0 netmask 255.255.255.0 {' >> /etc/dhcp/dhcpd.conf
echo '    option routers                  192.168.0.1;' >> /etc/dhcp/dhcpd.conf
echo '    option subnet-mask              255.255.255.0;' >> /etc/dhcp/dhcpd.conf
echo '    option broadcast-address        192.168.0.255;' >> /etc/dhcp/dhcpd.conf
echo '    range 192.168.0.40 192.168.0.254;' >> /etc/dhcp/dhcpd.conf
echo '}' >> /etc/dhcp/dhcpd.conf

echo -e '[Unit]' > /usr/lib/systemd/system/dhcp.service
echo -e 'Description=DHCPv4 Server Daemon' >> /usr/lib/systemd/system/dhcp.service
echo -e 'Documentation=man:dhcpd(8) man:dhcpd.conf(5)' >> /usr/lib/systemd/system/dhcp.service
echo -e 'Wants=network-online.target' >> /usr/lib/systemd/system/dhcp.service
echo -e 'After=network-online.target' >> /usr/lib/systemd/system/dhcp.service
echo -e 'After=time-sync.target' >> /usr/lib/systemd/system/dhcp.service
echo -e '' >> /usr/lib/systemd/system/dhcp.service
echo -e '[Service]' >> /usr/lib/systemd/system/dhcp.service
echo -e 'Type=notify' >> /usr/lib/systemd/system/dhcp.service
echo -e 'ExecStart=/usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid wlan0' >> /usr/lib/systemd/system/dhcp.service
echo -e '' >> /usr/lib/systemd/system/dhcp.service
echo -e '[Install]' >> /usr/lib/systemd/system/dhcp.service
echo -e 'WantedBy=multi-user.target' >> /usr/lib/systemd/system/dhcp.service

systemctl enable dhcpd.service
systemctl start dhcpd.service

reboot
```
