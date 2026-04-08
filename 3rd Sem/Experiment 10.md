### **Phase 1: Topology Setup**

*This topology is larger than the previous one. You are building two separate Local Area Networks (LANs) connected by a single Router.*

1.  **Place Devices:**
      * **1 x Router:** Select the **1941 Router**.
      * **2 x Switches:** Select **2960 Switches** (Place one on the left, one on the right).
      * **4 x PCs:** Place two PCs on the left (PC0, PC1) and two on the right (PC2, PC3).
2.  **Connect Devices:**
      * Use **Copper Straight-Through** cables.
      * **Left Side (Network A):** Connect PC0 and PC1 to **Switch0**. Connect **Switch0** to Router **GigabitEthernet0/0**.
      * **Right Side (Network B):** Connect PC2 and PC3 to **Switch1**. Connect **Switch1** to Router **GigabitEthernet0/1**.

-----

Diagram:

![Experiment 8 Topology Diagram](Experiment%2010.png)

-----


### **Phase 2: IP Addressing Strategy**

*Since we are connecting two **different** networks, we need two different IP ranges. We will use:*

  * **Network A (Left):** `192.168.1.x`
  * **Network B (Right):** `192.168.2.x`

**Addressing Table:**

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **PC0** | NIC | 192.168.1.2 | 255.255.255.0 | **192.168.1.1** |
| **PC1** | NIC | 192.168.1.3 | 255.255.255.0 | **192.168.1.1** |
| **Router** | **G0/0 (Left)** | **192.168.1.1** | **255.255.255.0** | N/A |
| **PC2** | NIC | 192.168.2.2 | 255.255.255.0 | **192.168.2.1** |
| **PC3** | NIC | 192.168.2.3 | 255.255.255.0 | **192.168.2.1** |
| **Router** | **G0/1 (Right)** | **192.168.2.1** | **255.255.255.0** | N/A |

-----

### **Phase 3: PC Configuration (Step 1)**

*Crucial: You MUST set the Default Gateway on every PC, or the ping will fail.*

1.  **Configure PC0 & PC1 (Left Side):**

      * Click **PC0** -\> **Desktop** -\> **IP Configuration**.
      * IP: `192.168.1.2`
      * Gateway: `192.168.1.1` (This points to the router's left arm).
      * *Repeat for PC1 using `.3`.*

2.  **Configure PC2 & PC3 (Right Side):**

      * Click **PC2** -\> **Desktop** -\> **IP Configuration**.
      * IP: `192.168.2.2`
      * Gateway: `192.168.2.1` (This points to the router's right arm).
      * *Repeat for PC3 using `.3`.*

-----

### **Phase 4: Router Configuration (Step 2)**

*We need to configure both "arms" of the router with the Gateway IPs we promised the PCs.*

1.  Click **Router0** -\> **CLI**.
2.  Type `no` if asked for the initial config dialog.
3.  Enter the following commands:

<!-- end list -->

```bash
! Enter Global Config
Router> enable
Router# configure terminal
Router(config)# hostname R1

! --- CONFIGURE LEFT NETWORK (Network A) ---
! Select the interface connected to the Left Switch
R1(config)# interface gigabitEthernet 0/0

! Assign the Gateway IP for the 192.168.1.x network
R1(config-if)# ip address 192.168.1.1 255.255.255.0

! Turn the port on (Red lights will turn green)
R1(config-if)# no shutdown
R1(config-if)# exit

! --- CONFIGURE RIGHT NETWORK (Network B) ---
! Select the interface connected to the Right Switch
R1(config)# interface gigabitEthernet 0/1

! Assign the Gateway IP for the 192.168.2.x network
R1(config-if)# ip address 192.168.2.1 255.255.255.0

! Turn the port on
R1(config-if)# no shutdown
R1(config-if)# exit

! --- SAVE ---
R1(config)# end
R1# write memory
```

-----

### **Phase 5: Verification (Step 3)**

*Now we verify if traffic can cross the router from Network A to Network B.*

1.  Open **PC0** (Left side).
2.  Open **Command Prompt**.
3.  Ping a PC on the *Right* side (e.g., PC2):
    `ping 192.168.2.2`
4.  **Expected Result:**
      * The first 1-2 lines might say "Request timed out" (this is ARP working).
      * The following lines should say **"Reply from 192.168.2.2..."**

**Troubleshooting:**

  * **"Destination host unreachable":** Usually means the PC does not have the correct **Default Gateway** configured. Check Phase 3.
  * **"Request timed out" (forever):** Usually means the Router interface is down (forgot `no shutdown`) or the IP on the router doesn't match the Gateway on the PC.