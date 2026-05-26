


# Lab 1: SD-WAN Topology Configuration

### NOTES FOR TOPOLOGY FLOW

1. Create config group
2. Create transport and mgmt profile
3. Attach interfaces / tracker
4. Review service profile
5. Deploy... whatever
6. Create config group for single router branch - preconfigured profiles - just review
7. Duplicate config group and items to build dual-router branch
8. Deploy, whatever
9. Topology full mesh vs hub and spoke
10. Insert verifications steps where necessary


In this lab, you will perform the following:

1. Create a configuration group that will be used to define the state of the DC routers.
2. Create a Transport & Management Profile to define VPN0 (transport) and VPN512 (management), and associate interfaces.
3. Create a Service Profile to define VPNs for the corporate and pci networks, and associate interfaces.
4. Deploy the datacenter configuration group to DC1 and DC2 routers.
5. Use a workflow to configure a branch with a single router.
6. Clone the single router branch configuration and deploy a dual router branch.
7. Create topologies for full mesh and hub and spoke, and observe the operational state differences of each.
8. Verify the transport and service VPNs are functioning as expected.

## Build the Datacenter Configuration

Configuration groups allow a structured, reusable way to group related configuration elements for a device (or group of devices) and then apply the rendered configuration to the devices in your topology. In this task, you will create a configuration group for the routers in datacenters DC1 and DC2.

### Task 1: Configuration Group

1. In the Cisco Catalyst SD-WAN Manager interface, click **Configuration** and then **Configuration Groups**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task01/step-01_configuration-groups.png" alt="Configuration Groups location" width="300px">

2. Click **Create Configuration Group** in the top navigation bar.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task01/step-02_create-group.png" alt="Create a new configuration group" width="300px">

3. Name the new group **dc_v01**. Add an optional description, set the Site Type to **Single Router**, then click **Create**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task01/step-03_group-details.png" alt="Creating the dc_v01 configuration group" width="300px">

4. In the dc_v01 configuration group, select **default_system_v01** for the System Profile. For Transport & Management Profile, click **+ Create New**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task01/step-04_complete_config_group.png" alt="dc_v01 System and Transport Profile association" width="300px">

5. In the "Add Feature Profile" dialog, name the new feature profile **dc_transport_v01** and, if desired, add a profile description. Click **Create** to add the new transport profile.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task01/step-05_add_feature_profile.png" alt="Adding a new feature profile" width="300px">


## Create the Transport & Management Profile

### Task 2: Transport and Management VPNs

1. Click **Select Transport VPN**, then click **+ Add New** to open the Transport VPN configuration context.
2. Name the profile **vpn0_profile** and add a profile description, if desired.
3. Locate "Enhance ECMP Keying", and use the drop-down to the left of the slider to select **Global**, then click the slider to the **enabled** position.

    !!! note
        Most configuration options allow you to select **Global**, **Device Specific**, or **Default**. For these options, the default is preconfigured. To change the default value, use **Global** if all devices assigned the profile will use the same setting (for example, ECMP Keying will be applied to every device using this profile). When **Device Specific** is used, you will be prompted to define a variable name for the field and to provide a value for that variable during the deployment phase. You will be instructed which option to use throughout this lab.

4. Define two global IPv4 DNS servers: **172.16.1.254** and **172.16.2.254**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-04_dns-ipv4-servers.png" alt="IPv4 DNS server definition" width="300px">

5. Locate "Routes" and click **+ Add IPv4 Static Route**.
6. Specify a default route. Enter **0.0.0.0** for Network address, and click the value **0.0.0.0** for Subnet Mask.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-06_default-route.png" alt="Create the default route" width="300px">

7. Scroll to the right in the IPv4 Static Route you just created. Observe that the value for Gateway is using the default of "nextHop". In the Next Hop column, click **+ Add** to define the next hop addresses.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-07_add-nexthop.png" alt="Add next hop addresses" width="300px">

8. In the Next Hop configuration dialog, select **Device Specific** and name the variable **vpn0_inet_gw**.
9. Click the menu icon to the right of the variable name. Enter a description such as **vpn0 - INET gateway** and set the default value to **172.16.1.254**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-09_inet-gw.png" alt="Adding the first gateway address" width="300px">

10. Click **+ Add Next Hop**. Click **Device Specific**, assign the variable **vpn0_mpls_gw**, and specify a description and default value of **172.16.2.254**. When complete, your next hop should match the Step Help.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-10_mpls-gw.png" alt="Adding the second gateway address" width="300px">

11. Click **Save** to save the Next Hop configuration.
12. Click **Save** to save the Transport VPN Profile. You will be returned to the dc_transport_v01 Transport & Management Profile dialog.
13. Click **+ Add New Feature**, then click **Management VPN** to add the management VPN (VPN 512).

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-13_create-management-vpn.png" alt="Add the Management VPN" width="300px">

14. In the Management VPN section, click **Select Management VPN**, then click **+ Add New**.
15. Set the Name to **vpn512_mgmt_profile** and add an optional description.
16. Add two **Global** IPv4 DNS servers, **172.16.1.254** and **172.16.2.254**. Your completed management VPN profile should closely match the Step Help.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task02/step-16_management-vpn.png" alt="Management VPN configuration" width="300px">

17. Click **Save** to save the Management VPN profile. You will return to the Transport & Management Profile configuration.

### Task 3: Add Interfaces to the Profiles

Assign interfaces and a tracking instance

1. In the Management VPN definition, click **+** and then click **Management Ethernet Interface**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-01_add_mgmt_interface.png" alt="Add a management ethernet interface" width="300px">

2. Under Management Ethernet Interface, click **Select Management Ethernet Interface** then click **+ Add New**

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-02_new_mgmt_interface.png" alt="Create a new interface" width="300px">

3. Name the interface profile **mgmt_interface_profile** and add an optional description, such as **Management interface profile**.

4. In the Basic Configuration section, define the following settings:

    - Shutdown: **Global**, **disabled**
    - Interface Name: **Global**, **GigabitEthernet8**
    - Description: **Global**, **>>> vpn512_mgmt <<<**

    When complete, your settings should match the Step Help.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-04_mgmt_interface_configuration.png" alt="Management interface configuration" width="300px">

5. Click **Save** to save the Management Ethernet Interface definition and return to the Transport & Management Profile.
6. In the Transport VPN definition, click **+** and then **Ethernet Interface**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-06_add-transport-interface.png" alt="Add a transport ethernet interface" width="300px">

7. Click **Select Ethernet Interface** and then click **+ Add New**.
8. Enter the name **vpn0_inet_interface_profile** and an optional description.
9. In the Basic Configuration section, complete the following items:

    - Shutdown: **Global**, **disabled**
    - Interface name: **Device Specific**, **vpn0_inet_interface**
    - Description: **Global**, **>>> vpn0_inet <<<**
    - IPv4 Settings:
        - Address Type: **Global**, **Static**
        - IP Address: **Device Specific**, **vpn0_inet_ip_address**
        - Subnet Mask: **Device Specific**, **vpn0_inet_subnet**

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-09_vpn0-inet-interface.png" alt="Internet interface basic configuration" width="300px">

10. Click **Tunnel** in the Ethernet Interface navigation bar.
11. Complete the interface Tunnel configuration, using the following settings:

    - Tunnel Interface: **Global**, **enabled**
    - Color: **Global**, **biz-internet**
    - NTP: **Global**, **enabled**
    - Encapsulation: **+ Add Encapsulation**
    - Encapsulation Settings:
        -  Encapsulation: **Global**, **ipsec**

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-11_inet-tunnel-config.png" alt="Internet interface tunnel configuration" width="300px">

12. Click **NAT** in the Ethernet Interface navigation bar.
13. Expand **IPv4 Settings**, then set NAT to **Global** and **enabled**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-13_enable-inet-nat.png" alt="Internet interface NAT configuration" width="300px">

14. Click **Save** to save the Ethernet Interface definition and return to the Transport & Management Profile settings.

15. Add a second Transport VPN Ethernet Interface by clicking **+** and then **Ethernet Interface**.
16. On the new Ethernet Interface, click **Select Ethernet Interface** and then **+ Add New**.
17. Repeat steps 8-14 above, using the following values to guide you:
    - Basic Configuration:
        - Profile Name: **vpn0_mpls_interface_profile**
        - Profile Description: Optional
        - Shutdown: **Global**, **disabled**
        - Interface name: **Device Specific**, **vpn0_mpls_interface** 
        - Description: **Global**, **>>> vpn0_mpls <<<**
        - IPv4 Settings:
            - Address Type: **Global**, **Static**
            - IP Address: **Device Specific**, **vpn0_mpls_ip_address**
            - Subnet Mask: **Device Specific**, **vpn0_mpls_subnet**
    - Tunnel:
        - Tunnel Interface: **Global**, **enabled**
        - Color: **Global**, **mpls**
        - NTP: **Global**, **enabled**
        - Encapsulation: **Global**, **ipsec**

    !!! note
        Do not enable NAT for the MPLS interface tunnel configuration!
18. Click **Save** to save the vpn0 MPLS interface profile and return the the Transport & Management Profile.
19. When you have created all interface definitions, your dc_transport_v01 profile should closely match the Step Help:

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task03/step-19_interfaces_added.png" alt="Interfaces added to the transport profile" width="300px">


### Task 4: Create a Tracking Instance

1. Locate the Ethernet Interface vpn0_inet_interface_profile, then click **+** and **Tracker**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task04/step-01_add-tracker.png" alt="Add a tracker" width="300px">

2. Under Tracker, click **Select Tracker** and then **+ Add New**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task04/step-02_create-tracker.png" alt="Create a new tracker" width="300px">

3. Complete the tracker configuration, using the below information. When complete, your tracker should closely match the Step Help.

    - Profile Name: **dia_tracker**
    - Profile Description: Optional
    - Tracker Name: **dia-tracker**
    - Endpoint: **Endpoint DNS Name**
    - DNS Name: **Global**, **google.com**
    - Probe Interval: **Global**, **30**
    - Number of Retries: **Global**, **2**

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task04/step-03_tracker-configuration.png" alt="DIA Tracker Configuration" width="300px">

4. Click **Save** to save the tracker configuration and return to the Transport & Management Profile. You should see the Tracker associated with the vpn0_inet_interface profile:

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task04/step-04_tracker-completed.png" alt="Completed tracker association" width="300px">

5. At the bottom of the dc_transport_v01 profile, click **Back** to return to the Configuration Group. You should now see the System and Transport & Management Profiles associated to the dc_v01 configuration group.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task04/step-05_updated-config-group.png" alt="Updated configuration group" width="300px">

    !!! note
        Save the dc_v01 configuration group at this time, if the **Save** button is illuminated.


## Review the Service Profile for Corporate and PCI VPNs

Service Profiles are used to define the overlay networks (VPNs) in the SD-WAN topology. In this task, a preconfigured Service Profile has been created for the DC routers to reduce the task burden for this four-hour lab. Follow the steps below to review the basics of service profiles, including VPN definitions, BGP configuration, and interface assignment. You will then attach the preconfigured profile to your dc_v01 configuration group before deploying the final configuration to the datacenter routers.

## Task 5: Create a Service Profile

1. In the Cisco Catalyst SD-WAN Manager, click **Configuration** and **Configuration Groups** to return to the Configuration Groups dialog.
2. In the top navigation menu, click on **Service Profile**
3. Locate the service profile named **default_dc_service_multi_vpn_01**, click the three dots to the right of the profile name, and click **Edit**. 

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task05/step-03_edit-service-profile.png" alt="Editing a service profile" width="300px">

4. Observe the following:
    - There are two Service VPNs created: Corporate (VPN10) and PCI (VPN11).
    - Each Service VPN definition contains two Ethernet Interface definitions and a single BGP Routing definition.

5. Click the **edit** icon for the first Service VPN, Corporate. **SCREEN CAPTURE NEEDED ONCE NAME DETERMINED**
6. In the Service VPN configuration dialog that appears, scroll through the options and observe the following:

    - The VPN ID is **10**. The VPN ID is used as the VRF ID on routers participating in the SD-WAN topology.
    - OMP will advertise connected, static, BGP, and injected (network) routes. No IPv6 advertisements have been configured.
    - No additional settings have been changed from the default values in the Service VPN profile.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task05/step-06_service-vpn-settings.png" alt="Service VPN configuration settings" width="300px">

7. Click **Cancel** to exit the Service VPN configuration dialog without saving any unexpected changes.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task05/step-07_cancel-button.png" alt="Cancel the Service VPN configuration" width="300px">

8. Click the **edit** icon for the Corporate VPN "lan" Ethernet Interface.
9. Observe that the interface definition is minimal. Only a Global interface name, description, and a Device Specific IPv4 address has been defined. **SCREEN CAPTURE NEEDED WHEN FINALIZED VARIABLES**
10. Click **Cancel** to exit the Ethernet Interface configuration without saving any changes.
11. Locate the BGP Routing instance for the Corporate VPN, and click the **edit** icon.
12. Observe the following: **SCREEN CAPTURE NEEDED**

    - A Global AS Number has been defined.
    - The number of BGP maximum-paths has been globally defined as 2.
    - BGP is configured to redistribute routes learned from OMP.
    - One Device Specific IPv4 BGP neighbor has been defined.

13. Click **Cancel** to close the BGP Routing configuration context without saving any changes.
14. Click **Configuration** and then **Configuration Groups** to return to the Configuration Group dialog.


## Task 6: Complete and Deploy the Configuration Group

1. Click the **dc_v01** configuration group to expand the settings.
2. Click **+ Add Profile** and then click **Service Profile**

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-02_add-service-profile.png" alt="Add a service profile" width="300px">

3. For Service Profile, click **Select One** and then click **default_dc_service_multi_vpn_v01** to attach the preconfigured Service Profile.
4. Click **+ Add Profile** and then click **CLI Add-on Profile**.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-04_add-cli-profile.png" alt="Add a CLI profile" width="300px">

5. For CLI Add-on Profile, click **Select One** and then click **edge_basic_cli**.

    !!! note
        The preconfigured CLI profile contains a single command, `platform console serial`.

6. When complete, your dc_v01 Configuration Group should match the Step Help:

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-06_completed-config-group.png" alt="Completed Configuration Group" width="300px">

7. Click **Save** to save the completed Configuration Group.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-07_save-config-group.png" alt="Save the configuration group" width="300px">

8. In the Configuration Group "Deployment" section, observe that no devices are associated. Click **+ Add** to associate devices with the configuration group.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-08_add-devices.png" alt="Associating devices with the configuration group" width="300px">

9. Select **dc1** and **dc2** in the left navigation pane to associate the datacenter routers with the configuration group, then click **Save**.

    !!! note
        You might be warned that the devices are already associated with another Configuration Group. Click **Confirm** to move the devices to your dc_v01 Configuration Group.

    ??? tip "Step Help"
        <img src="../assets/images/lab01/task06/step-09_add-dc-routers.png" alt="Add the routers in dc1 and dc2" width="300px">

10. Observe that the dc_v01 Configuration Group now has four associated devices, and all four are out of sync. "Out of sync" indicates that the device configuration does not match that defined in the Configuration Group.
11. 



Corporate and PCI Networks

### Steps:

## Legacy steps below:
