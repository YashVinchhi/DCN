# Experiment 8

-----

### **Phase 1: Topology Setup**

1.  **Open Cisco Packet Tracer.**
2.  **Add Devices:**
      * Select **Switches** from the bottom left menu and drag a **2960 Switch** to the workspace.
      * Select **End Devices** and drag two **Laptops** (Laptop0 and Laptop1).
3.  **Connect Devices:**
      * Select **Connections** (lightning bolt icon).
      * Choose the **Copper Straight-Through** cable (solid black line).
      * Click **Laptop0** -\> Select **FastEthernet0**.
      * Click **Switch0** -\> Select **FastEthernet0/1**.
      * Repeat for **Laptop1**, connecting it to **FastEthernet0/2** on the switch.

-----

Diagram:

![Experiment 8 Topology Diagram](Experiment%208.png)

-----

### **Phase 2: Assign IP Addresses (Step 1)**

*You need to give the laptops addresses so they can talk to each other.*

1.  **Configure Laptop0:**
      * Click on **Laptop0**.
      * Go to the **Desktop** tab -\> **IP Configuration**.
      * **IPv4 Address:** `192.168.1.1`
      * **Subnet Mask:** `255.255.255.0`
2.  **Configure Laptop1:**
      * Click on **Laptop1**.
      * Go to the **Desktop** tab -\> **IP Configuration**.
      * **IPv4 Address:** `192.168.1.2`
      * **Subnet Mask:** `255.255.255.0`

-----

### **Phase 3: Switch Configuration (Step 2)**

*This addresses the "Scenario" requirements: securing the CLI, setting passwords (encrypted & plain), and setting a banner.*

1.  Click on the **Switch**.
2.  Go to the **CLI** tab.
3.  Press `Enter` to get started.
4.  Type the following commands (lines starting with `!` are comments to help you understand, do not type them):

<!-- end list -->

```bash
Switch> enable

Switch# configure terminal

Switch(config)# hostname S1


S1(config)# enable password cisco

S1(config)# enable secret class


S1(config)# line console 0

S1(config-line)# password consolepass

S1(config-line)# login

S1(config-line)# exit


S1(config)# banner motd #WARNING: Authorized Access Only!#


S1(config)# service password-encryption

S1(config)# end

```

-----

### **Phase 4: Verification / Ping (Step 3)**

*Now verify connectivity between the host devices.*

1.  Click on **Laptop0**.
2.  Go to the **Desktop** tab -\> **Command Prompt**.
3.  Type the ping command to reach Laptop1:
    `ping 192.168.1.2`
4.  **Success Criteria:** You should see replies (e.g., `Reply from 192.168.1.2...`).
      * *Note:* The first ping might fail (Request timed out) while ARP resolves. If the next 3 are successful, the experiment is complete.

-----