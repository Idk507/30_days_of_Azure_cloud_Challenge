### **Understanding CIDR (Classless Inter-Domain Routing) in Detail**  

CIDR (Classless Inter-Domain Routing) is a method used for IP address allocation and routing. It **replaces the traditional class-based system (Class A, B, C)** with a more efficient and flexible system. CIDR helps in:  

✅ **Efficient IP Address Allocation** – Reduces wastage of IPs  
✅ **Hierarchical Routing** – Improves routing efficiency  
✅ **Scalability** – Allows ISPs and enterprises to manage networks better  

---

## **1. IP Address Structure**
An **IP address** consists of **32 bits (IPv4)**, divided into 4 octets (8 bits each). Each octet is separated by a dot (`.`) and represented in decimal.  

Example:  
`192.168.1.0` → Binary: `11000000.10101000.00000001.00000000`

Each IP consists of:  
- **Network Portion** (identifies the network)
- **Host Portion** (identifies devices inside the network)

---

## **2. What is CIDR Notation?**
CIDR uses a **slash (`/`) followed by a number** to represent the **subnet mask**. This number **defines how many bits belong to the network portion**.  

👉 **Example**: `192.168.1.0/24`  
- `/24` means **24 bits for the network, leaving 8 bits for hosts**.  
- The subnet mask is **255.255.255.0**  
- Hosts available: **2⁸ - 2 = 254** usable IPs (`.1 to .254`)

📌 **CIDR Notation Format:**  
```
IP Address / Subnet Mask Length
```
---

## **3. CIDR Subnetting Table**
| CIDR | Subnet Mask | Total IPs | Usable Hosts | Example Range |
|------|------------|-----------|--------------|--------------|
| `/8`  | 255.0.0.0  | 16,777,216 | 16,777,214 | `10.0.0.0 - 10.255.255.255` |
| `/16` | 255.255.0.0 | 65,536 | 65,534 | `192.168.0.0 - 192.168.255.255` |
| `/24` | 255.255.255.0 | 256 | 254 | `192.168.1.0 - 192.168.1.255` |
| `/30` | 255.255.255.252 | 4 | 2 | `192.168.1.0 - 192.168.1.3` |

📌 **Formula for Usable Hosts:**  
\[
2^{(32 - \text{CIDR})} - 2
\]
(Since 2 IPs are reserved: **1 for network, 1 for broadcast**)

---

## **4. CIDR Block Aggregation (Supernetting)**
CIDR allows **combining multiple networks into one** using supernetting.  

Example:  
- Instead of four **/24** subnets: `192.168.0.0/24, 192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24`
- Use **one /22 subnet**: `192.168.0.0/22` (covers all four)  
**Advantage**: Reduces routing table entries.

---

## **5. CIDR Visual Representation**
Here’s a **flowchart** to understand CIDR subnetting and IP allocation:

```
                  +----------------------+
                  |   IP Address (32-bit) |
                  +----------------------+
                             |
                             v
                +-------------------------+
                | CIDR Notation (e.g., /24)|
                +-------------------------+
                             |
                             v
       +--------------------------------+
       | Network Portion / Host Portion |
       +--------------------------------+
               /            \
      +------+             +------+
      | Network Address     | Usable Hosts (2^n - 2)
      +------+             +------+
```

### **Visual Representation of CIDR Block:**
For `/24` → 254 usable IPs  

```
192.168.1.0/24
---------------------------------------------------
| Network |      Usable IPs       | Broadcast |
|  .0     | .1   -   .254         |   .255   |
---------------------------------------------------
```

For `/30` → Only 2 usable IPs  

```
192.168.1.0/30
-------------------------------------
| Network | Usable IPs | Broadcast |
|   .0    | .1  -  .2  |    .3     |
-------------------------------------
```

---

## **6. CIDR Example in Networking**
### **Scenario: Company Network Setup**
A company has **three departments** and wants to allocate **separate subnets**:

| Department | Required Hosts | Suggested CIDR | Usable IPs |
|------------|---------------|---------------|-----------|
| HR         | 50            | /26          | 62        |
| IT         | 120           | /25          | 126       |
| Finance    | 25            | /27          | 30        |

**Subnet Allocation:**
- **HR**: `192.168.1.0/26` → Usable: `192.168.1.1 - 192.168.1.62`
- **IT**: `192.168.1.128/25` → Usable: `192.168.1.129 - 192.168.1.254`
- **Finance**: `192.168.1.64/27` → Usable: `192.168.1.65 - 192.168.1.94`

---

## **7. CIDR Benefits**
✅ **Reduces IP Wastage** – Avoids unnecessary large subnets  
✅ **Improves Routing Efficiency** – Aggregates multiple subnets  
✅ **Enhances Security** – Fine-grained control over IP allocation  
✅ **Scalable** – Allows easy expansion  

---

### **Final Summary**
- CIDR **replaces traditional class-based networking**.
- It defines IP addresses using **prefix length (/x notation)**.
- Allows **flexible subnetting** to efficiently use IP addresses.
- CIDR enables **aggregation (supernetting) to optimize routing tables**.

