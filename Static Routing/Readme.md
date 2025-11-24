# Static Routing Lab – Configuring End-to-End Connectivity

### Network Topology  
*(Packet Tracer Diagram)*  
<img src="networkdiagram_staticrouting" width="85%" />

---

## **Description**
This project demonstrates how I configured a 3-router network using **static routing** to enable communication between:

- **PC1 → 192.168.1.1 (LAN 1)**
- **PC2 → 192.168.3.1 (LAN 2)**

The objective was to manually configure all interfaces, default gateways, and static routes so packets travel across R1 → R2 → R3 and back successfully.

This lab helps understand:
- How routers forward packets via **next hops**
- How to configure **static routes**
- Relationship between **connected, local, and static** routes
- End-to-end network connectivity troubleshooting

---

## **Technologies Used**
- **Packet Tracer**

---

## **Environment**
- Cisco Routers (R1, R2, R3)
- Cisco Switches (SW1, SW2)
- PCs running simulated Windows CLI (Packet Tracer)
- Subnets used:
  - 192.168.1.0/24
  - 192.168.12.0/24
  - 192.168.13.0/24
  - 192.168.3.0/24

---

# **Project Walkthrough**

---

##  **1. PC1 Configuration**
```
IP Address:       192.168.1.1
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.254  (R1 G0/1)
```

---

##  **2. R1 Configuration**

```bash
en
conf t
hostname R1

interface g0/1
 ip address 192.168.1.254 255.255.255.0
 description ## To SW1 ##
 no shutdown

interface g0/0
 ip address 192.168.12.1 255.255.255.0
 description ## To R2 ##
 no shutdown
```

---

##  **3. R2 Configuration**

```bash
en
conf t
hostname R2

interface g0/0
 ip address 192.168.12.2 255.255.255.0
 description ## To R1 ##
 no shutdown

interface g0/1
 ip address 192.168.13.2 255.255.255.0
 description ## To R3 ##
 no shutdown
```

---

##  **4. R3 Configuration**

```bash
en
conf t
hostname R3

interface g0/0
 ip address 192.168.13.3 255.255.255.0
 description ## To R2 ##
 no shutdown

interface g0/1
 ip address 192.168.3.254 255.255.255.0
 description ## To SW2 ##
 no shutdown
```

---

##  **5. PC2 Configuration**
```
IP Address:       192.168.3.1
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.3.254  (R3 G0/1)
```

---

#  Static Routing

Routers only know **directly connected** networks.  
To reach remote networks, we configure **manual static routes**.

##  **R1 Static Route (towards PC2)**

```bash
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

---

## **R2 Static Routes (middle router)**

```bash
ip route 192.168.3.0 255.255.255.0 192.168.13.3
ip route 192.168.1.0 255.255.255.0 192.168.12.1
```

---

## **R3 Static Route (towards PC1)**

```bash
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

---

# Verification

### **Check routing tables**
```bash
show ip route
```

Expect:
- **C** – Connected networks  
- **L** – Local addresses  
- **S** – Static routes  

---

### **Ping Test From PC1**
```
ping 192.168.3.1
```

✔ First ping fails due to ARP  
✔ Following pings succeed  

---

# Final Result

You now have:
- Fully working static routing
- End-to-end communication between PC1 and PC2
- Correct routing tables on all routers
- Correct gateway configuration on PCs

---

