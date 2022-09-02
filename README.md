# ansible-ocp  

bastion/helper node는 forward/reverse lookup이 가능해야 함.(같은 domain 사용할 것 )  
```  
nmcli con add type ethernet ifname ens224 con-name ens224
nmcli con mod ens224 ipv4.method manual "192.168.150.1"
nmtui-edit ens224

nmcli connection modify ens224 connection.zone internal
nmcli connection modify ens192 connection.zone external

firewall-cmd --zone=external --add-masquerade --permanent
firewall-cmd --zone=internal --add-masquerade --permanent

firewall-cmd --reload

firewall-cmd --list-all --zone=internal
firewall-cmd --list-all --zone=external

cat /proc/sys/net/ipv4/ip_forward
```  