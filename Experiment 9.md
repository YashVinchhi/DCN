# Experiment 9

### **Phase 1: Topology Setup**

*Construct the network as shown in the diagram.*

1.  **Devices Needed:**
      * **Router:** Select the **1941 Router** (Round icon with arrows).
      * **Switch:** Select a **2960 Switch**.
      * **End Devices:** Select two **PCs**.
2.  **Connections:**
      * Use **Copper Straight-Through** cables (solid black line).
      * Connect **PC0** FastEthernet0 $\rightarrow$ **Switch** FastEthernet0/1.
      * Connect **PC1** FastEthernet0 $\rightarrow$ **Switch** FastEthernet0/2.
      * Connect **Switch** GigabitEthernet0/1 $\rightarrow$ **Router** GigabitEthernet0/0.

-----

Diagram:

![Experiment 8 Topology Diagram](Experiment%209.png)

-----

### **Phase 2: IP Configuration (Step 1)**

*Unlike the switch experiment, routers require an IP address on their interface to function as a Gateway.*

**1. Configure PC0:**

  * **IP Address:** `192.168.1.2`
  * **Subnet Mask:** `255.255.255.0`
  * **Default Gateway:** `192.168.1.1` *(This will be the Router's address)*

**2. Configure PC1:**

  * **IP Address:** `192.168.1.3`
  * **Subnet Mask:** `255.255.255.0`
  * **Default Gateway:** `192.168.1.1`

-----

### **Phase 3: Router Configuration (Step 2)**

*This is the core of the experiment: setting up the router interface and securing access.*

1.  Click on the **Router**.
2.  Go to the **CLI** tab.
3.  If asked *"Would you like to enter the initial configuration dialog?"*, type **no** and press Enter.
4.  Type the following commands:

<!-- end list -->

```bash
! Enter Privileged Mode
Router> enable

! Enter Global Configuration Mode
Router# configure terminal

! Set Hostname
Router(config)# hostname R1

! --- 1. CONFIGURE ROUTER INTERFACE (To make the network work) ---
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
! (You should see green lights on the link now)

! --- 2. SET ENABLE PASSWORD (Plain text) ---
! Used when typing 'enable' to get administrative rights
R1(config)# enable password cisco

! --- 3. SET ENABLE SECRET (Encrypted) ---
! This overrides the 'enable password' and is more secure
R1(config)# enable secret class

! --- 4. SET CONSOLE PASSWORD ---
! Secures the physical port
R1(config)# line console 0
R1(config-line)# password consolepass
R1(config-line)# login
R1(config-line)# exit

! --- 5. SET VTY PASSWORD (Telnet) ---
! Secures remote access via the network
R1(config)# line vty 0 4
R1(config-line)# password vtypass
R1(config-line)# login
R1(config-line)# exit

! --- 6. ENCRYPT ALL PASSWORDS ---
! This hides plain text passwords (like 'cisco' and 'consolepass') in the config
R1(config)# service password-encryption

! Exit to verify
R1(config)# end
```

-----

### **Phase 4: Verification (Step 3)**

*Now you must prove the security works.*

**1. Verify Console Security:**

  * Press `Enter` in the Router CLI. You should be kicked out or see a prompt.
  * It should ask for a password.
  * Type: `consolepass` (Characters will not appear while typing).
  * **Success:** You get the `R1>` prompt.

**2. Verify Enable Security:**

  * Type `enable`.
  * It will ask for a password.
  * Type: `class` (The "secret" password always takes precedence over the standard "password").
  * **Success:** You get the `R1#` prompt.

**3. Verify Configuration Encryption:**

  * Type: `show running-config`
  * Look at your passwords. They should look like random numbers (e.g., `7 0822455D0A16`) instead of "cisco" or "consolepass".

**4. Verify VTY (Remote) Access:**

  * Open **PC0** $\rightarrow$ **Command Prompt**.
  * Type: `telnet 192.168.1.1`
  * It should ask for the VTY password.
  * Type: `vtypass`.

**Would you like an explanation of the difference between `enable password` and `enable secret` for your viva/exam prep?**