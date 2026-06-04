# Section Four – Secure Access

## Secure Access


1. Open the Cisco Secure Access: Zero Trust Access Lab v1 by clicking the View button in the dcloud interface
2. In the dCloud Session View, click the Launch Panel tab
3. Click View under the Cisco Secure Access option
4. In the Secure Access interface, click Admin and then click API Keys under Management
5. Click the add button on the right
6. Input the API Key name (SDWAN_Manager), check the Deployments box and then click Create Key
7. Make note of the organization ID for your Secure Access tenant by looking at the URL in your browser.  The number between /org/ and /admin/ in that URL is the org ID you’ll need in the next steps.
8. Save the Generated API Key by copying and pasting them into a text file, once saved, click ACCEPT AND CLOSE
9. Return to SD-WAN Manager, click Administration and then Settings
10. In settings, navigate to Cloud Credentials, input the required infromatino for the Cisco SSE integration:
11. Flip the Cisco SSE toggle to blue
12. Input the Organization ID
13. Input the API Key and the Secret
14. Flip the Context Sharing toggle to blue
15. Click Save
16. Return to the Secure Access web interface, in the left hand menu click on Connect and then click Network Connections, then click on the Network Tunnel Groups tab.
17. Click the +Add button in the bottom right.
18. Input the General Settings for your Network Tunnel Group
19. Tunnel Group Name = CatalystSDWANBranches
20. Region = US Pacific Northwest
21. Device Type = Catalyst SDWAN
22. Click Next
23. In the Tunnel ID and Passphrase dialog, input the required informations
24. Tunnel ID = CatalystSDWANTunnels
25. Passphrase = CiscoSecure8000Routers Confirm Passphrase = CiscoSecure800Routers
26. Click Next
27. Input the details for the Routing options and network overlaps
28. Click the Static routing radio button
29. In IP address Ranges input 10.10.0.0/16, 10.11.0.0/16 and click Add
30. Click Save
31. The next screen displays the configuration details – save this data by downloading the CSV
32. Return to Catalyst SD-WAN Manager
33. Navigate to the Configuration Group for the Single Router Branches
34. Edit the Transport and Management Profile
35. Click Add Feature next to the top level option in the Transport VPN (should be named VPN10)
36. Click Ethernet Interface in the options presented to the right, click it twice so that two Ethernet Interfaces are created awaiting configuration.
37. Configure the two Ethernet Interfaces with the following details
38. Name = SSE-Loopback0 & SSE-Loopback1
39. Interface name = Loopback0 & Loopback1
40. IP Address should be set to static
41. Set the options to Device specific for the IP address and mask
42. For the IP Address variable, name them SSE_Loopback0IPaddr & SSE_Loopback1IPaddr
43. For the IP address description input “SSE_Lo0 10.1.X.1 or SSE_Lo1 10.1.X.2”
44. For the Subnetmask variable input SSE_Lo0Mask & SSE_Lo1Mask
45. For the Subnetmask description input ‘Loopback Mask is /32’
46. For the Subnet mask default value choose /32:255.255.255.255
47. Save those configurations
48. Deploy the configurations to the Single Router Branch routers (Br1 & Br2. via the ‘out of sync’ button that should show 2 routers out of sync
49. In the Deploy Configuration Group dialog, input the new IP Address information for the new Loopback addresses on each router:
50. Br1-edge1: Lo0 = 10.1.3.1/32 Lo1 = 10.1.3.2/32
51. Br2-edge2: Lo0 = 10.1.4.1/32 Lo1 = 10.1.4.2/32
52. Click next, then click deploy and then click view the deployment status to monitor the deployment for errors.
53. Click Configuration on the left, then click Policy Groups
54. Click the Secure Internet Gateway/ Secure Service Edge tab
55. Click Add Secure Internet Access
56. Name the new policy ‘SRB_SSE_Policy_v01
57. Choose the solution as SDWAN
58. Click Create
59. Choose the Cisco SSE radio button under SSE Provider, and flip the VPN toggle to blue for Context Sharing.
60. Click Add Tracker just below the source IP address field and input the details required for this dialog:
61. Name = SSE-Tracker
62. API URL = https://www.cisco.com
63. Click Add
64. In the Configuration Section, click +Add Tunnel, you will do this twice – once for each Looback.
65. Interface name = ipsec1 & ipsec2
66. Tunnel Source Interface = Loopback0 & Looback1
67. Tunnel Route Via = GigabitEthernet1
68. Tracker = SSE-Tracker
69. Data Center = Primary
70. Click add
71. In the High Availability section click +Add Interface Pair and input the required details – do this twice, once for each previously created ipsec interface
72. Active interface = ipsec1 & ipsec2
73. Backup Interface = none
74. Active Interface Weight = 1
75. Back Interface Weight = 1
76. Click save in the bottom right
77. Click the Application Priority & SLA Policy tab
78. In the next screen, click Application Priority and SLA Policy to create a new one
79. Name is SRB_SSE_Redirect_v01 and click Create
80. In top right there is a toggle for Advanced Layout, clip that toggle to the right so it appears blue
81. Click +Add Traffic Policy and input the required details:
82. Policy Name = SRB_SSE_Redirect_Policy_v01
83. VPN(s. = VPN10
84. Direction = all
85. Default Action = Accept
86. Click add
87. Click +Add rules
88. In the rule section change the base action to Accept and then click +Add Match
89. In the match action dialog, scroll down to Secure Internet Gateway/Secure Service Edge and check that box
90. Select the Secure Service Edge Radio button
91. Select the Cisco Secure Access in the drop-down menu
92. Check Fall back to routing
93. Click Save match and actions
94. Validate your configuration shows the following:
95. Base action = accept
96. Failback to Routing = true
97. Secure Service Edge = sse
98. Secure Service Edge Instance = Cisco-Secure-Access
99. Click Save in the bottom right.
100. Navigate back to the Policy Group configuration page by clicking Configuration on the left and then clicking Policy Groups.
101. Click Add Policy Group and input the required information:
102. Policy Group Name = SRB_SSE_PolicyGroup_v01
103. Solution =sdwan
104. Click Create
105. In the new Policy Group populate the drop downs with the details we created in the last few steps:
106. Application Priority = SRB_SSE_Redirect_v01
107. Secure Internet Access / Secure Internet Gateway = SRB_SSE_Policy_v01
108. NGFW = NGFW_Policy_v01 (optional but keeps the existing configuration we did in the last section)
109. Click Save
110. Once Saved, click the edit pencil next to Associate to associate the policy to the two Single Router Branch routers.
111. Do not deploy the new configuration yet – first we must validate the way that traffic is flowing from the br1-vpn10-pc and br2-vpn10-pc before we apply the policy.
112. Return to the Cisco Modeling Labs web interface and right click on the br1-vpn10-pc and then click VNC, in the window that opens at the bottom, click Open VNC
113. Log into the PCs with the password admin
114. Click on the globe icon in the bottom menu bar to open a web browswer.
115. In the web browser, navigate to www.whatismyipaddress.com and notice that the location displayed should show the local that the lab is hosted in (San Jose, Research Triangle Park, London, Sydney or Singapore)
116. Return to Catalyst SD-WAN Manager and deploy the policy to the routers with the Deploy button
117. Input the loopback0 IP Address for each router as part of the deployment process
118. Monitor the deployment for errors
119. In Catalyst SD-WAN Manager, click Monitor in the left-hand menu and then click Tunnels. Then click the SIG/SSE Tunnels tab.
120. Here you should be able to monitor the tunnels coming into an UP state – it will take a few minutes.
121. Once the tunnels show up, navigate back to the CML Interface and log back into the VNC clients for the pcs
122. Refresh the web page to see how the location changes showing the new exit point validating that the traffic is being redirected through Secure Access.
123. Go back to the Secure Access web interface and in the left-hand menu, click Monitor and then click Activity Search and you should see data that shows what is coming from these branch networks into the Secure Access environment.