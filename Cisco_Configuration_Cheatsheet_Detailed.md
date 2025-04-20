# 🛠️ Cisco Configuration Cheatsheet (Detailed)

This cheatsheet provides detailed configuration commands for common Cisco networking tasks.

---

## 1. Configuring Hostname
Sets the device name used in prompts and network identification.
```bash
configure terminal
hostname Router1
end
```

---

## 2. Configuring Domain Name
Sets the domain name required for SSH key generation.
```bash
configure terminal
ip domain-name example.com
end
```

---

## 3. Setting Passwords

### Enable Secret (Encrypted)
```bash
configure terminal
enable secret cisco123
end
```

### Console Access Password
```bash
configure terminal
line console 0
 password console123
 login
end
```

### VTY (SSH/Telnet) Access Password
```bash
configure terminal
line vty 0 4
 password vtypass
 login
end
```

---

## 4. Adding Users to Local Database
Creates a local user with a privilege level.
```bash
configure terminal
username admin privilege 15 secret adminpass
end
```

---

## 5. AAA with SSH Authentication
Sets up local user login via SSH with AAA.
```bash
configure terminal
aaa new-model
aaa authentication login default local
ip domain-name example.com
crypto key generate rsa modulus 2048
ip ssh version 2

line vty 0 4
 login authentication default
 transport input ssh
end
```

---

## 6. VLAN Configuration
Creates a VLAN with a name.
```bash
configure terminal
vlan 10
 name SALES
exit
end
```

---

## 7. Assigning Interfaces to VLANs
Assigns an interface to a VLAN and sets access mode.
```bash
configure terminal
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
end
```

---

## 8. Generating RSA Key Pairs for SSH
RSA key is required for enabling SSH.
```bash
configure terminal
crypto key generate rsa modulus 2048
end
```

---

## 9. AAA SSH Connection Summary
Step-by-step for secure SSH login using local database.
1. Set hostname
2. Set domain name
3. Enable AAA with `aaa new-model`
4. Create user with privilege 15
5. Generate RSA keys (2048 bits)
6. Enable SSH version 2
7. Apply AAA to VTY lines

---

## 10. Setting the Default Gateway (Router)
Used in Layer 2 switches or routers in specific setups.
```bash
configure terminal
ip default-gateway 192.168.1.1
end
```

---

## 11. VPN Setup (IPSec Site-to-Site)
```bash
configure terminal
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
end
```

---

## 12. Spanning Tree Protocol (STP)
### Enable RPVST+ and secure edge ports
```bash
configure terminal
spanning-tree mode rapid-pvst

interface FastEthernet0/1
 spanning-tree portfast
 spanning-tree bpduguard enable

spanning-tree vlan 1 priority 24576
end
```

---

## 13. Zone-Based Policy Firewall (ZPF)
```bash
configure terminal
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
end
```

---

✅ This cheatsheet is tailored for hands-on configuration of Cisco devices. Adapt IPs and names to your own lab or production environment.
