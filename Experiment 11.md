### **Part 1: DHCP Server (Dynamic Host Configuration Protocol)**
*Goal: Make the server automatically give IP addresses to the PCs so you don't have to type them manually.*

**1. Topology Setup:**
* **Devices:** 1 Server (Generic), 1 Switch (2960), 2 PCs.
* **Connections:** Connect everything with Straight-Through cables.

**2. Server Configuration:**
* Click on the **Server**.
* **Static IP Setup:** Go to **Desktop** $\rightarrow$ **IP Configuration** and assign the Server's IP:
    * **IP Address:** `192.168.1.1`
    * **Subnet Mask:** `255.255.255.0`
* **Enable DHCP Service:**
    * Go to **Services** tab $\rightarrow$ select **DHCP** (from the left menu).
    * **Service:** Click **On**.
    * **Pool Name:** `server pool` (or leave default).
    * **Start IP Address:** `192.168.1.2` (This is where it starts counting).
    * **Subnet Mask:** `255.255.255.0`
    * **Maximum Number of Users:** `3`
    * **Click SAVE.** (Important: Do not click "Add", click "Save" to update the default pool).

**3. Client Verification:**
* Go to **PC0** $\rightarrow$ **Desktop** $\rightarrow$ **IP Configuration**.
* Switch the radio button from **Static** to **DHCP**.
* **Success:** It should say "DHCP Request Successful" and populate the IP as `192.168.1.2`.

---

### **Part 2: DNS Server (Domain Name System)**
*Goal: Allow users to type "www.rku.in" instead of "192.168.1.1" to access a website.*

**1. Server Configuration:**
* Click on the same **Server**.
* Go to **Services** tab $\rightarrow$ select **DNS**.
* **Service:** Click **On**.
* **Name:** `www.rku.in`
* **Address:** `192.168.1.1` (We are pointing this domain to the server itself).
* **Click ADD.**

**2. Client Verification:**
* Go to **PC0**.
* **Configure DNS Server IP:** In **IP Configuration**, ensure the **DNS Server** field is set to `192.168.1.1` (You may need to switch back to Static to edit this, or update your DHCP pool settings in the server).
* **Test:** Open the **Web Browser** on the PC.
* Type `www.rku.in` and press Go.
* **Success:** You should see the default "Cisco Packet Tracer" web page.

---

### **Part 3: Email Server**
*Goal: Configure two users ("komal" and "krishna") to send emails to each other.*

**1. Server Configuration:**
* Click on the **Server**.
* Go to **Services** tab $\rightarrow$ select **EMAIL**.
* **SMTP Service:** On
* **POP3 Service:** On
* **Domain Name:** `gmail.com` $\rightarrow$ Click **Set**.
* **Create Users:**
    * User 1: `komal` | Password: `123` $\rightarrow$ Click **+** (Add).
    * User 2: `krishna` | Password: `456` $\rightarrow$ Click **+** (Add).

**2. PC Configuration (Mail Client Setup):**
* **Configure Komal (PC0):**
    * Open **PC0** $\rightarrow$ **Desktop** $\rightarrow$ **Email**.
    * **Your Name:** Komal
    * **Email Address:** `komal@gmail.com`
    * **Incoming Mail Server:** `192.168.1.1`
    * **Outgoing Mail Server:** `192.168.1.1`
    * **Username:** `komal`
    * **Password:** `123`
    * Click **Save**.

* **Configure Krishna (PC1):**
    * Open **PC1** $\rightarrow$ **Desktop** $\rightarrow$ **Email**.
    * **Your Name:** Krishna
    * **Email Address:** `krishna@gmail.com`
    * **Incoming/Outgoing Server:** `192.168.1.1`
    * **Username:** `krishna`
    * **Password:** `456`
    * Click **Save**.

**3. Verification (Sending Email):**
* On **PC0 (Komal)**, click **Compose**.
* **To:** `krishna@gmail.com`
* **Subject:** `Test`
* **Body:** `Hello Krishna`
* Click **Send**.
* On **PC1 (Krishna)**, click **Receive**.
* **Success:** You should see the new email appear in the list.