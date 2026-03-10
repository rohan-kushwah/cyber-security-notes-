Hey! Could you clarify what you mean by "reserved IP"? Are you referring to:

1. **Reserved IP in DHCP** – assigning a specific IP address to a device based on its MAC address.
    
2. **Reserved IP address ranges** – like private IP ranges (e.g., `192.168.0.0/16`, `10.0.0.0/8`, `172.16.0.0/12`).
    
3. **Reserved public IP** – e.g., in cloud services like AWS/GCP, a static public IP you assign to a VM or resource.
    
4. Something else?

  ip changing setting dns public dns etc etc priavte dns working of dhcp and dns


Got it! You’re talking about reserving an IP address for a device using its **MAC address**—typically done on a **DHCP server**. This ensures the device always gets the same IP address.

Here’s a quick guide depending on what system you're using:

---

### 💻 **On Windows Server DHCP**

1. Open **DHCP Manager** (`dhcpmgmt.msc`).
    
2. Navigate to your scope:  
    `IPv4 > [Your Scope] > Reservations`
    
3. Right-click **Reservations** > **New Reservation**.
    
4. Fill in:
    
    - **Reservation Name**: Anything meaningful.
        
    - **IP Address**: The one you want to reserve.
        
    - **MAC Address**: No dashes or colons (e.g., `001122334455`)
        
    - **Description**: Optional.
        
    - **Supported types**: Usually **Both (BOOTP and DHCP)**.
        
5. Click **Add**.