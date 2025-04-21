
# Cisco Configuration Cheat Sheet with Examples

---

## 1. Set Hostname & Domain Name
```bash
enable
configure terminal
hostname R1
ip domain-name example.com
```

## 2. Set Enable Secret (Encrypted)
```bash
enable secret cisco123
```

## 3. Configure Local User Account
```bash
username admin privilege 15 secret adminpass
```

## 4. Configure Console Access
```bash
line console 0
password conpass
login
exit
```

## 5. Configure VTY (SSH) Access
```bash
line vty 0 4
login local
transport input ssh
```

## 6. Enable AAA for SSH Authentication
```bash
aaa new-model
aaa authentication login default local
```

## 7. Generate RSA Keys (For SSH)
```bash
crypto key generate rsa
# Enter 1024 or 2048 when prompted
ip ssh version 2
```

## 8. Assign IP Address to Interface
```bash
interface gigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

## 9. Create VLAN and Assign Ports (on Switch)
```bash
vlan 10
name Users
exit
interface range fa0/1 - 10
switchport mode access
switchport access vlan 10
exit
```

## 10. Set Default Gateway (on Switch)
```bash
interface vlan 10
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
```

## 11. Save Configuration
```bash
write memory
# or
copy running-config startup-config
```

## 12. Verify Configuration
```bash
show running-config
show ip interface brief
show vlan brief
show users
```
