# 🛠️ Cisco Configuration Cheatsheet

## 1. Configuring Hostname
```
hostname Router1
```

## 2. Configuring Domain Name
```
ip domain-name example.com
```

## 3. Setting Passwords
```
enable secret cisco123

line console 0
 password console123
 login

line vty 0 4
 password vtypass
 login
```

## 4. Adding Users to Local Database
```
username admin privilege 15 secret adminpass
```

## 5. AAA with SSH Authentication
```
aaa new-model
aaa authentication login default local
ip domain-name example.com
crypto key generate rsa modulus 2048
ip ssh version 2

line vty 0 4
 transport input ssh
 login authentication default
```

## 6. VLAN Configuration
```
vlan 10
 name SALES
```

## 7. Assigning Interfaces to VLANs
```
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
```

## 8. Generating RSA Key Pairs for SSH
```
crypto key generate rsa modulus 2048
```

## 9. AAA SSH Connection Summary
```
1. Set hostname and domain name
2. Enable aaa new-model
3. Create local user
4. Configure SSH version, RSA key
5. Use login authentication on VTY lines
```

## 10. Setting the Default Gateway (Router)
```
ip default-gateway 192.168.1.1
```

## 11. VPN Setup (IPSec Site-to-Site)
```
crypto isakmp policy 10
 encr aes
 hash sha
 authentication pre-share
 group 2

crypto isakmp key cisco123 address 192.168.2.1

crypto ipsec transform-set TS esp-aes esp-sha-hmac

crypto map VPN-MAP 10 ipsec-isakmp
 set peer 192.168.2.1
 set transform-set TS
 match address 100

interface GigabitEthernet0/0
 crypto map VPN-MAP

access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
```

## 12. Spanning Tree Protocol (STP)
```
spanning-tree mode rapid-pvst

interface FastEthernet0/1
 spanning-tree portfast
 spanning-tree bpduguard enable

spanning-tree vlan 1 priority 24576
```

## 13. Zone-Based Policy Firewall (ZPF)
```
zone security IN
zone security OUT

interface GigabitEthernet0/0
 zone-member security IN

interface GigabitEthernet0/1
 zone-member security OUT

class-map type inspect match-any HTTP_TRAFFIC
 match protocol http

policy-map type inspect HTTP_POLICY
 class type inspect HTTP_TRAFFIC
  inspect

zone-pair security ZP-IN-OUT source IN destination OUT
 service-policy type inspect HTTP_POLICY
```
