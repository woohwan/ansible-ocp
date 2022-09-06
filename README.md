# ansible-ocp  
vSphere 상에서 OVA와 metal방식으로 Openshift 설치 테스트  


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
- ansible 수행 시 환경 설정  (rhel 8) -- 전제 시스템에 적용  
firewalld 문제는 case by case --> ansible.cfg에서 python_interpreter 삭제 --> Ok   
pyvmomi 인식 못함 (/usr/libexec/no-python) --> 해당  playbook  실행 시  command에 -e "ansible_python_interpreter=/usr/bin/python" 덧붙여 수행.  
openshift_vsphere role의 vars directory 에 ansible_python_interpreter: /usr/bin/python 추가  
```   
  yum install python3.9
  alternatives --set python /usr/bin/python3.9
  alternatives --set pip /usr/bin/pip3.9
```    
admin으로 login
```  
  pip install ansible
  pip install ansible-lint
  ansible-galaxy collection install ansible.posix
```  

- Baremetal에 pxe를 사용해서 openshift 설치 시 pxelinux.0 대신 lpxelinux.0를 사용.  
- pxelinux.cfg 에 매뉴에서 매뉴얼대로 kernel, initramfs, rootfs 사용.  
