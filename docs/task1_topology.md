# Configure the Topology

You should have 8 Catalyst 8000v routers onboarded to the Catalyst SD-WAN Manager.  

Two Datacenter ‘Hubs’ are configured each with DC1 & DC2 routers (4 total). Each Data Center Hub router has two WAN connections, one to the ‘MPLS’ Network and the other to the ‘Internet’ network. 

There are three branches to configure in this network.  Two are single router branches with and ‘MPLS’ and an ‘Internet’ type WAN connection. The third branch is a dual router branch with two Edge routers, each with a WAN connection. In the Dual Router Branch, since these routers don’t both have a WAN connection into each WAN network – you will need to use a TLOC Extension to provide redundancy between them for this branch. 

All of these routers have a very basic configuration applied to them that essentially simply onboards them to the Catalyst SD-WAN Controllers. Their WAN interfaces are configured so that they can communicate with the control components, but no routing is configured. No topology is defined. That’s your job – configure this network.  

The Lab topology is simple, two data centers with two hub routers each and three branch sites with simulated WAN connectivity to provide the underlay. 

The WAN connectivity is labeled MPLS and INET but could be a representation of any type of WAN connectivity. MPLS and INET are just a label and the only functional different between them is that the underlying lab components are configured to provide NAT’ed access to the Internet via the GATEWAY router for the INET WAN connections. You will use that path for the Direct Internet Access (DIA)requirements. 

## Task 1: Configure the Transport and Management VPNs

1. Access the Cisco Catalyst SD-WAN Manager web console, using your browser. Use the URL provided in the **resources** section.
2. In the left menu, click **Configuration** and then **Configuration Groups**.
3. Click **Create Configuration Group**.
4. Complete the new configuration group settings, using the being information to guide you:
   5. Name: **dc_v01**
   6. Site Type: **Single Router**
5. Complete the dc_v01 Configuration Group. Select **default_system_v01** for the System Profile, and for Transport & Management Profile, click **Create New**.
6. In the "Add Feature Profile" dialog, assign the name **dc_transport_v01** then click **Create**.
7. Click **Select Transport VPN** then click **+ Add New**.
8. Complete the Transport VPN using the following information to guide you:
   9. Name: **wan_vpn**
   10. Enhance ECMP Keying: Select **Global** then **enable** the slider.
   11. Add DNS IPv4: **enabled**
   11. Primary DNS Address (IPv4): Select **Global** and enter IP address **172.16.1.254**
   12. Secondary DNS Address (IPv4): Select **Global** and enter IP address **172.16.2.254**
   13. IPv4 Static Routes: **0.0.0.0/0.0.0.0**. Gateway should remain unchanged.
   14. For the IPv4 static route, click **+ Add** for Next Hop to open the Next Hop dialog.
   15. Select **Device Specific** and change the template variable name to **VPN0_INET_DefaultGW**.
   16. Click the menu to the right of the address. For Description, enter **INET WAN GATEWAY IP** and for Default value, enter **172.16.1.254**.
   17. Click **+ Add Next Hop** to add a second address.
   18. Change the second next hop to **Device Specific** and set the variable name to **VPN0_MPLS_DefaultGW**. Add the description **MPLS WAN GATEWAY IP** and set the default value to **172.16.2.254**.
   19. Click **Save** to save the static route entry.
   20. Click **Save** to save the wan_vpn Transport VPN Profile.
21. On the dc_v01 Transport & Management Profile, click **+ Add New Feature**, then click **Management VPN**.
22. Click **Select Management VPN** then click **+ Add New**.
23. Click the slider for **Add DNS IPv4** and create the Global primary and secondary DNS addresses, **172.16.1.254** and **172.16.2.254**.
24. Click **Save** to save the Management VPN profile.
25. When completed, your dc_v01 Transport and Management Profile should appear as follows:

## Assign Interfaces to the Management and Transport VPNs

1. Click **+** on the right side of the Management VPN definition, then click **Management Ethernet Interface**.
2. Click **Select Management Ethernet Interface** then click **+ Add New**.
3. Name the interface **MGMT_Interface** and specify the interface name **GigabitEthernet8**. *Note: the interface name must match exactly, or the configuration will not be applied!*
4. Click **Save** to save the Management Ethernet Interface.
5. 
6. 
7. 
8. Repeat the above steps for the Transport VPN Ethernet Interface. Use the following information to guide you:
   6. Name: **INET_Interface**
   7. Shutdown: Global, **disabled**
   8. Interface name: Device Specific, variable name **VPN0_INET_Interface**
   9. Click the menu to the right of the interface name. For Description, enter **vpn0 INET interface** and set the default value to **GigabitEthernet1**
   9. Description: Global, assign a meaningful value e.g. **>>> INET VPN interface <<<**
   10. IPv4 Settings:
       11. Address Type: **Static**
       12. IP Address: Device Specific, variable name **VPN0_INET_IPAddress**
       13. Subnet Mask: Device Specific, variable name **VPN0_INET_Subnet**
   11. Click **Tunnel** to enter the tunnel configuration context.
   12. For Tunnel Interface, select **Global** and slide to **enabled**.
   13. Color: Global, **dia-internet**
   14. Locate **NTP** and select **Global** and slide to **enabled**.
   15. Navigate to Encapsulation and click **+ Add Encapsulation**
   16. For Encapsulation, select **ipsec**. Leave Preference and Weight at the default values, then click **Add**.
   17. In the top navigation menu, click **NAT**
   18. Click **IPv4 Settings**. For **NAT**, select **Global** and move the slider to **enabled**.
   19. Click **Save** to save the INET Ethernet Interface definition.
6. On the Transport VPN, click **+** for the Transport VPN and click **Ethernet Interface**.
7. Click **+ Add New** for the new Transport VPN Ethernet Interface. Repeat the above steps, using the following information to guide you:
   8. Name: **MPLS_vpn_interface**
   9. Shutdown: **disabled**
   10. Interface name: Device Specific, variable name **VPN0_MPLS_Interface**
   11. Set a default value with description for the interface name. Description: **vpn0 MPLS interface**, default value **GigabitEthernet2**.
   11. Description: Global, **>>> MPLS VPN interface <<<**
   12. IPv4 Settings:
       13. Address Type: **Static**
       14. IP Address: Device Specific, variable **VPN0_MPLS_IPAddress**
       15. Subnet Mask: Device Specific, variable **VPN0_MPLS_Subnet**
   16. NTP: Global, **enabled**
   17. Tunnel interface: **Global**, **enabled**
   18. Tunnel color: **mpls**
   18. Encapsulation: **ipsec**
   19. *DO NOT* configure NAT for the MPLS interface.
8. When complete, your Transport and Management Profile should match the following:
9. Locate the INET_Interface definition for the wan_vpn, and click **+** to add a feature. Click **Tracker**.
10. Click **Select Tracker**, then click **+ Add New**
11. Configure the Tracker as follows:
    12. Name: **dia_tracker**
    13. Tracker Name: **dia-tracker**
    14. Endpoint: **Endpoint DNS Name**
    15. DNS Name: Global, **google.com**
    16. Probe Interface: Global, **30**
    17. Number of Retries: Globa, **2**
12. Click **Save** to save the Tracker definition.
13. At the top of the Transport and Management Profile, click **<- Feature Profiles** to return to the configuration group.

## Add the Service and CLI Profiles

Service and CLI profiles have already been created for you. In this task, you will associate the Service and CLI Add-on Profiles to the dc_v01 configuration group. The profiles define (something). You will be prompted to enter necessary values in the following task, "Deploy the Configuration"

1. On the Configuration Groups page, click **Service Profile** then click **Add New**.
2. Select the preexisting profile named **default_dc_service_multi_vpn_v01**
3. Click **Add New** for CLI profile
4. Select the preexisting profile named **default_branch_cli_v01**


## Configure the CLI Add-on Profile

1. Do something

## Deploy the Configuration

1. Click **Save** to save the dc_v01 Configuration Group.
2. Click **+ Add** to associate devices to the configuration group.
3. In the list of Devices, **select** each router in sites dc1 and dc2.
4. Click **Save**. *Note: You may receive a warning that the devices are already associated. Click **Confirm** to associate the devices with your configuration template.*
5. Click **Deploy** to begin the configuration deployment process.
6. Select all Edge Devices on the deployment dialog, then click **Next**.
7. 

## Task 2: Full Mesh vs. Hub and Spoke

