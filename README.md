# best_LoadBalancing_realIP


# mar/02/2026 13:47:08 by RouterOS 6.49.6
# software id = FLLA-NJBB
#
# model = 951Ui-2HnD
# serial number = 558104BE41E5
/interface bridge
add name=bridge1
/interface ethernet
set [ find default-name=ether1 ] comment=Main
set [ find default-name=ether2 ] comment=Backup
/interface wireless
set [ find default-name=wlan1 ] ssid=MikroTik
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=dhcp_pool0 ranges=192.168.1.2-192.168.1.254
/ip dhcp-server
add address-pool=dhcp_pool0 disabled=no interface=bridge1 name=dhcp1
/interface bridge port
add bridge=bridge1 interface=ether3
add bridge=bridge1 interface=ether4
add bridge=bridge1 interface=ether5
/ip address
add address=172.30.1.194/30 comment=Onesky_Uplink_Main interface=ether1 \
    network=172.30.1.192
add address=172.30.1.198/30 comment=Onesky_Uplink_Backup interface=ether2 \
    network=172.30.1.196
add address=192.168.1.1/24 comment=Lan interface=bridge1 network=192.168.1.0
add address=123.253.38.105/29 comment=Lan interface=bridge1 network=\
    123.253.38.104
add address=103.91.130.125/30 comment=Lan interface=bridge1 network=\
    103.91.130.124
/ip dhcp-server network
add address=10.0.0.0/24 gateway=10.0.0.1
add address=192.168.1.0/24 gateway=192.168.1.1
/ip dns
set servers=8.8.8.8
/ip firewall address-list
add address=123.253.38.104/29 list=Public
add address=103.91.130.124/30 list=Public
/ip firewall nat
add action=masquerade chain=srcnat src-address-list=!Public
/ip route
add check-gateway=ping distance=1 gateway=172.30.1.193
add check-gateway=ping distance=2 gateway=172.30.1.197
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www port=8080
set ssh disabled=yes
set api disabled=yes
set winbox port=8505
set api-ssl disabled=yes
/system clock
set time-zone-name=Asia/Dhaka
/system identity
set name="Best Electronics"
