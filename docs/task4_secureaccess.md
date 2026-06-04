# Section Four – Secure Access

1. Open the Cisco Secure Access: Zero Trust Access Lab v1 by clicking the View button in the dcloud interface
2. In the dCloud Session View, click the Launch Panel tab
3. Click View under the Cisco Secure Access option
4. In the Secure Access interface, click Admin and then click API Keys under Management

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step04.png" width="600px" alt="step"/>

5. Click the add button on the right

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step05.png" width="600px" alt="step"/>

6. Input the API Key name (SDWAN_Manager), check the Deployments box and then click Create Key

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step06.png" width="600px" alt="step"/>

7. Save the Generated API Key by copying and pasting them into a text file, once saved, click ACCEPT AND CLOSE

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step07.png" width="600px" alt="step"/>

8. Make note of the organization ID for your Secure Access tenant by looking at the URL in your browser.  The number between /org/ and /admin/ in that URL is the org ID you’ll need in the next steps.
    !!! Note
        You can get the organization ID for your Secure Access Tenant by looking at the address in the browser address bar, for example:
        https://dashboard.sse.cisco.com/org/8405576/admin/apikeys
        In this example the number ‘8405576’ would be the Organization ID for this instance. 
9. Return to SD-WAN Manager, click Administration and then Settings
10. In settings, navigate to Cloud Credentials, input the required infromation for the Cisco SSE integration:

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step10a.png" width="600px" alt="step"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step10b.png" width="600px" alt="step"/>

    ```
        Flip the Cisco SSE toggle to blue
        Input the Organization ID
        Input the API Key and the Secret
        Flip the Context Sharing toggle to blue
    ```

11. Click Save
12. Return to the Secure Access web interface, in the left hand menu click on Connect and then click Network Connections, then click on the Network Tunnel Groups tab.

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step12.png" width="600px" alt="step"/>

13. Click the +Add button in the bottom right.

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step13.png" width="600px" alt="step"/>

14. Input the General Settings for your Network Tunnel Group

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step14.png" width="600px" alt="step"/>

    ```
       Tunnel Group Name = CatalystSDWANBranches
       Region = US Pacific Northwest
       Device Type = Catalyst SDWAN
    ```
    
15. Click Next
16. In the Tunnel ID and Passphrase dialog, input the required informations

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step16.png" width="600px" alt="step"/>

    ```
        Tunnel ID = CatalystSDWANTunnels
        Passphrase = CiscoSecure8000Routers
        Confirm Passphrase = CiscoSecure8000Routers
    ```
    
17. Click Next
18. Input the details for the Routing options and network overlaps

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step18.png" width="600px" alt="step"/>

19. Click the Static routing radio button
20. In IP address Ranges input 10.10.0.0/16, 10.11.0.0/16 and click Add

21. Click Save
22. The next screen displays the configuration details – save this data by downloading the CSV

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step20.png" width="600px" alt="step"/>

23. Return to Catalyst SD-WAN Manager
24. Navigate to the Configuration Group for the Single Router Branches
25. Edit the Transport and Management Profile
26. Click Add Feature next to the top level option in the Transport VPN (should be named VPN0)
27. Click Ethernet Interface in the options presented to the right, click it twice so that two Ethernet Interfaces are created awaiting configuration.
28. Configure the two Ethernet Interfaces with the following details

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step25a.png" width="600px" alt="step"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step25b.png" width="600px" alt="step"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step25c.png" width="600px" alt="step"/>

    ```
        Name = SSE-Loopback0 & SSE-Loopback1
        Interface name = Loopback0 & Loopback1
        IP Address should be set to static
        Set the options to Device specific for the IP address and mask
        For the IP Address variable, name them SSE_Loopback0IPaddr & SSE_Loopback1IPaddr 
        For the IP address description input “SSE_Lo0 10.1.X.1 or SSE_Lo1 10.1.X.2” 
        For the Subnetmask variable input SSE_Lo0Mask & SSE_Lo1Mask
        For the Subnetmask description input ‘Loopback Mask is /32’
        For the Subnet mask default value choose /32:255.255.255.255
    ```
    
29. Save those configurations
30. Deploy the configurations to the Single Router Branch routers (Br1 & Br2. via the ‘out of sync’ button that should show 2 routers out of sync)
31. In the Deploy Configuration Group dialog, input the new IP Address information for the new Loopback addresses on each router:

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step31a.png" width="600px" alt="step"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab05/sse_step31b.png" width="600px" alt="step"/>

    ```
        Br1-edge1: Lo0 = 10.1.3.1/32 Lo1 = 10.1.3.2/32
        Br2-edge2: Lo0 = 10.1.4.1/32 Lo1 = 10.1.4.2/32
    ```
    
32. Click next, then click deploy and then click view the deployment status to monitor the deployment for errors.
33. Click Configuration on the left, then click Policy Groups
34. Click the Secure Internet Gateway/ Secure Service Edge tab
35. Click Add Secure Internet Access 

    ```
        Name the new policy ‘SRB_SSE_Policy_v01
        Choose the solution as SDWAN
    ```
    
36. Click Create
37. Choose the Cisco SSE radio button under SSE Provider, and flip the VPN toggle to blue for Context Sharing.
38. Click Add Tracker just below the source IP address field and input the details required for this dialog:

    ```
       Source IP Address (device Specific) = TrackerLo0Address
       Name = SSE-Tracker
       API URL = http://www.cisco.com
    ```
    
39. Click Add
40. In the Configuration Section, click +Add Tunnel, you will do this twice – once for each Looback.

    ```
       Interface name = ipsec1 & ipsec2
       Tunnel Source Interface = Loopback0 & Looback1
       Tunnel Route Via = GigabitEthernet1
       Tracker = SSE-Tracker
       Data Center = Primary
    ```

41. Click add
42. In the High Availability section click +Add Interface Pair and input the required details – do this twice, once for each previously created ipsec interface

    ```
       Active interface = ipsec1 & ipsec2
       Backup Interface = none
       Active Interface Weight = 1
       Backup Interface Weight = 1
    ```
    
43. Click save in the bottom right
44. Click the Application Priority & SLA Policy tab
45. In the next screen, click Application Priority and SLA Policy to create a new one
46. Name is SRB_SSE_Redirect_v01 and click Create
47. In top right there is a toggle for Advanced Layout, clip that toggle to the right so it appears blue
48. Click +Add Traffic Policy and input the required details:
49. Policy Name = SRB_SSE_Redirect_Policy_v01

    ```
       VPN(s. = VPN10
       Direction = all
       Default Action = Accept
    ```
    
50. Click add
51. Click +Add rules
52. In the rule section change the base action to Accept and then click +Add Match
53. In the match action dialog, scroll down to Secure Internet Gateway/Secure Service Edge and check that box
54. Select the Secure Service Edge Radio button
55. Select the Cisco Secure Access in the drop-down menu
56. Check Fall back to routing
57. Click Save match and actions
58. Validate your configuration shows the following:

    ```
       Base action = accept
       Failback to Routing = true
       Secure Service Edge = sse
       Secure Service Edge Instance = Cisco-Secure-Access
    ```
    
59. Click Save in the bottom right.
60. Navigate back to the Policy Group configuration page by clicking Configuration on the left and then clicking Policy Groups.
61. Click Add Policy Group and input the required information:

    ```
       Policy Group Name = SRB_SSE_PolicyGroup_v01
       Solution =sdwan
    ```
    
62. Click Create
63. In the new Policy Group populate the drop downs with the details we created in the last few steps:

   ```
      Application Priority = SRB_SSE_Redirect_v01
      Secure Internet Access / Secure Internet Gateway = SRB_SSE_Policy_v01
      NGFW = NGFW_Policy_v01 (optional but keeps the existing configuration we did in the last section)
   ```
   
64. Click Save
65. Once Saved, click the edit pencil next to Associate to associate the policy to the two Single Router Branch routers.
66. Do not deploy the new configuration yet – first we must validate the way that traffic is flowing from the br1-vpn10-pc and br2-vpn10-pc before we apply the policy.
67. Return to the Cisco Modeling Labs web interface and right click on the br1-vpn10-pc and then click VNC, in the window that opens at the bottom, click Open VNC
68. Log into the PCs with the password admin
69. Click on the globe icon in the bottom menu bar to open a web browser.
70. In the web browser, navigate to www.whatismyipaddress.com and notice that the location displayed should show the local that the lab is hosted in (San Jose, Research Triangle Park, London, Sydney or Singapore)
71. Return to Catalyst SD-WAN Manager and deploy the policy to the routers with the Deploy button
72. Input the loopback0 IP Address for each router as part of the deployment process
73. Monitor the deployment for errors
74. In Catalyst SD-WAN Manager, click Monitor in the left-hand menu and then click Tunnels. Then click the SIG/SSE Tunnels tab.
75. Here you should be able to monitor the tunnels coming into an UP state – it will take a few minutes.
76. Once the tunnels show up, navigate back to the CML Interface and log back into the VNC clients for the pcs
77. Refresh the web page to see how the location changes showing the new exit point validating that the traffic is being redirected through Secure Access.
78. Go back to the Secure Access web interface and in the left-hand menu, click Monitor and then click Activity Search and you should see data that shows what is coming from these branch networks into the Secure Access environment.
