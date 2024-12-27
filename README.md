### README: Router on a Stick with Spanning Tree Protocol Configuration

---

#### **Overview**
This project demonstrates a "Router on a Stick" setup using the Spanning Tree Protocol (STP) in Rapid-PVST mode. The topology includes VLAN segmentation, trunking, and dynamic IP configuration through DHCP. The network ensures redundancy, scalability, and load-balancing capabilities for VLAN traffic. 

---

#### **Topology Details**
- **Switches:** Configured in Rapid-PVST mode with designated root bridges per VLAN.
- **Router:** Configured with sub-interfaces for inter-VLAN routing.
- **VLANs:**
  - VLAN 700 (Blue): 10.7.0.0/24
  - VLAN 800 (Pink): 10.8.0.0/24
  - VLAN 900 (Green): 10.9.0.0/24
- **Spanning Tree Root Bridges:**
  - S2: Root Bridge for VLAN 700
  - S7: Root Bridge for VLAN 800
  - S6: Root Bridge for VLAN 900
- **VTP:** Implemented for centralized VLAN configuration management.

---

#### **Configuration Steps**

1. **Hostname and Rapid-PVST Activation**
   ```bash
   Switch(config)#hostname S1
   S1(config)#spanning-tree mode rapid-pvst
   ```

2. **Access Ports and PortFast**
   Assign VLANs to access ports and enable PortFast for edge devices:
   ```bash
   S1(config)#interface range FastEthernet 0/1-8
   S1(config-if-range)#switchport mode access
   S1(config-if-range)#switchport access vlan 700
   S1(config-if-range)#spanning-tree portfast
   ```

   Repeat for VLAN 800 and VLAN 900 with corresponding interfaces.

3. **Trunk Ports**
   Enable trunking on inter-switch links:
   ```bash
   S1(config)#interface range GigabitEthernet 0/1-2
   S1(config-if-range)#switchport mode trunk
   ```

4. **Root Bridge Configuration**
   Set primary root bridges:
   ```bash
   S2(config)#spanning-tree vlan 700 root primary
   S7(config)#spanning-tree vlan 800 root primary
   S6(config)#spanning-tree vlan 900 root primary
   ```

5. **VTP Configuration**
   Configure VLANs and VTP domain:
   ```bash
   S7(config)#vtp mode server
   S7(config)#vtp domain Cyber
   S7(config)#vtp password cyber
   ```

   Create VLANs and assign names:
   ```bash
   S7(config)#vlan 700
   S7(config-vlan)#name Blue
   S7(config)#vlan 800
   S7(config-vlan)#name Pink
   S7(config)#vlan 900
   S7(config-vlan)#name Green
   ```

6. **Router on a Stick**
   Configure router sub-interfaces for VLANs:
   ```bash
   R1(config)#interface FastEthernet 0/0.700
   R1(config-subif)#encapsulation dot1Q 700
   R1(config-subif)#ip address 10.7.0.1 255.255.255.0
   ```

   Repeat for VLAN 800 and VLAN 900.

7. **DHCP Configuration**
   Set up DHCP pools for dynamic IP assignment:
   ```bash
   R1(config)#ip dhcp pool Vlan700
   R1(dhcp-config)#network 10.7.0.0 255.255.255.0
   R1(dhcp-config)#default-router 10.7.0.1
   ```

   Repeat for VLAN 800 and VLAN 900.

---

#### **Features**
- **Dynamic VLAN Segmentation:** Ensures traffic isolation.
- **Redundancy via STP:** Prevents loops and optimizes active paths.
- **Centralized VLAN Management:** VTP simplifies VLAN configuration.
- **Dynamic IP Assignment:** DHCP automates IP management.

---

