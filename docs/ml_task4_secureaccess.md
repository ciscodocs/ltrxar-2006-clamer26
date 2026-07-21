Secure Access Steps
1.  Open the Cisco Secure Access: Zero Trust Access Lab v1 by clicking the View button in the dcloud interface
2.  In the dCloud Session View, click the Launch Panel tab
3)	Click View under the Cisco Secure Access option
4)	In the Secure Access interface, click Admin and then click API Keys under Management
5)	Click the add button on the right
6)	Input the API Key name (SDWAN_Manager), check the Deployments box and then click Create Key
7)	Make note of the organization ID for your Secure Access tenant by looking at the URL in your browser.  The number between /org/ and /admin/ in that URL is the org ID you’ll need in the next steps.
8)	Save the Generated API Key by copying and pasting them into a text file, once saved, click ACCEPT AND CLOSE
9)	Return to SD-WAN Manager, click Administration and then Settings
10)	In settings, navigate to Cloud Credentials, input the required infromatino for the Cisco SSE integration:
a.	Flip the Cisco SSE toggle to blue
b.	Input the Organization ID
c.	Input the API Key and the Secret
d.	Flip the Context Sharing toggle to blue
e.	Click Save
11)	Return to the Secure Access web interface, in the left hand menu click on Connect and then click Network Connections, then click on the Network Tunnel Groups tab.
12)	Click the +Add button in the bottom right.
13)	Input the General Settings for your Network Tunnel Group
a.	Tunnel Group Name = CatalystSDWANBranches
b.	Region = US Pacific Northwest
c.	Device Type = Catalyst SDWAN
d.	Click Next
14)	In the Tunnel ID and Passphrase dialog, input the required informations
a.	Tunnel ID = CatalystSDWANTunnels
b.	Passphrase = CiscoSecure8000Routers
c.	Confirm Passphrase = CiscoSecure8000Routers
d.	Click Next
15)	Input the details for the Routing options and network overlaps
a.	Click the Static routing radio button
b.	In IP address Ranges input 10.10.0.0/16, 10.11.0.0/16 and click Add
c.	Click Save
d.	The next screen displays the configuration details – save this data by downloading the CSV
16)	Return to Catalyst SD-WAN Manager
17)	Navigate to the Configuration Group for the Single Router Branches
18)	Edit the Transport and Management Profile
19)	Click Add Feature next to the top level option in the Transport VPN (should be named VPN10)
20)	Click Ethernet Interface in the options presented to the right, click it twice so that two Ethernet Interfaces are created awaiting configuration.
21)	Configure the two Ethernet Interfaces with the following details
a.	Name = SSE-Loopback0 & SSE-Loopback1
b.	Interface name = Loopback0 & Loopback1
c.	IP Address should be set to static
d.	Set the options to Device specific for the IP address and mask
e.	For the IP Address variable, name them SSE_Loopback0IPaddr & SSE_Loopback1IPaddr
f.	For the IP address description input “SSE_Lo0 10.1.X.1 or SSE_Lo1 10.1.X.2”
g.	For the Subnetmask variable input SSE_Lo0Mask & SSE_Lo1Mask
h.	For the Subnetmask description input ‘Loopback Mask is /32’
i.	For the Subnet mask default value choose /32:255.255.255.255
22)	Save those configurations
23)	Deploy the configurations to the Single Router Branch routers (Br1 & Br2) via the ‘out of sync’ button that should show 2 routers out of sync
24)	In the Deploy Configuration Group dialog, input the new IP Address information for the new Loopback addresses on each router:
a.	Br1-edge1: Lo0 = 10.1.3.1/32 Lo1 = 10.1.3.2/32
b.	Br2-edge2: Lo0 = 10.1.4.1/32 Lo1 = 10.1.4.2/32
25)	Click next, then click deploy and then click view the deployment status to monitor the deployment for errors.
26)	Click Configuration on the left, then click Policy Groups
27)	Click the Secure Internet Gateway/ Secure Service Edge tab
28)	Click Add Secure Internet Access
29)	Name the new policy ‘SRB_SSE_Policy_v01
30)	Choose the solution as SDWAN 
31)	Click Create
32)	Choose the Cisco SSE radio button under SSE Provider, and flip the VPN toggle to blue for Context Sharing.
33)	Click Add Tracker just below the source IP address field and input the details required for this dialog:
a.	Name = SSE-Tracker
b.	API URL = https://www.cisco.com
c.	Click Add
34)	In the Configuration Section, click +Add Tunnel, you will do this twice – once for each Looback.
a.	Interface name = ipsec1 & ipsec2
b.	Tunnel Source Interface = Loopback0 & Looback1
c.	Tunnel Route Via = GigabitEthernet1
d.	Tracker = SSE-Tracker
e.	Data Center = Primary
f.	Click add
35)	In the High Availability section click +Add Interface Pair and input the required details – do this twice, once for each previously created ipsec interface
a.	Active interface = ipsec1 & ipsec2
b.	Backup Interface = none
c.	Active Interface Weight = 1
d.	Back Interface Weight = 1
36)	 Click save in the bottom right
37)	Click the Application Priority & SLA Policy tab
38)	In the next screen, click Application Priority and SLA Policy to create a new one
39)	Name is SRB_SSE_Redirect_v01 and click Create
40)	In top right there is a toggle for Advanced Layout, clip that toggle to the right so it appears blue
41)	Click +Add Traffic Policy and input the required details:
a.	Policy Name = SRB_SSE_Redirect_Policy_v01
b.	VPN(s) = VPN10
c.	Direction = all
d.	Default Action = Accept
e.	Click add
42)	Click +Add rules
43)	In the rule section change the base action to Accept and then click +Add Match
44)	In the match action dialog, scroll down to Secure Internet Gateway/Secure Service Edge and check that box
45)	Select the Secure Service Edge Radio button
46)	Select the Cisco Secure Access in the drop-down menu
47)	Check Fall back to routing
48)	Click Save match and actions
49)	Validate your configuration shows the following:
a.	Base action = accept
b.	Failback to Routing = true
c.	Secure Service Edge = sse
d.	Secure Service Edge Instance = Cisco-Secure-Access
50)	Click Save in the bottom right.
51)	Navigate back to the Policy Group configuration page by clicking Configuration on the left and then clicking Policy Groups.
52)	Click Add Policy Group and input the required information:
a.	Policy Group Name = SRB_SSE_PolicyGroup_v01
b.	Solution =sdwan
c.	Click Create
53)	In the new Policy Group populate the drop downs with the details we created in the last few steps:
a.	Application Priority = SRB_SSE_Redirect_v01
b.	Secure Internet Access / Secure Internet Gateway = SRB_SSE_Policy_v01
c.	NGFW = NGFW_Policy_v01 (optional but keeps the existing configuration we did in the last section)
d.	Click Save
54)	Once Saved, click the edit pencil next to Associate to associate the policy to the two Single Router Branch routers.
55)	Do not deploy the new configuration yet – first we must validate the way that traffic is flowing from the br1-vpn10-pc and br2-vpn10-pc before we apply the policy.
56)	Return to the Cisco Modeling Labs web interface and right click on the br1-vpn10-pc and then click VNC, in the window that opens at the bottom, click Open VNC
57)	Log into the PCs with the password admin
58)	Click on the globe icon in the bottom menu bar to open a web browswer. 
59)	In the web browser, navigate to www.whatismyipaddress.com and notice that the location displayed should show the local that the lab is hosted in (San Jose, Research Triangle Park, London, Sydney or Singapore)
60)	Return to Catalyst SD-WAN Manager and deploy the policy to the routers with the Deploy button
61)	Input the loopback0 IP Address for each router as part of the deployment process
62)	Monitor the deployment for errors
63)	In Catalyst SD-WAN Manager, click Monitor in the left-hand menu and then click Tunnels. Then click the SIG/SSE Tunnels tab.
64)	Here you should be able to monitor the tunnels coming into an UP state – it will take a few minutes.
65)	Once the tunnels show up, navigate back to the CML Interface and log back into the VNC clients for the pcs
66)	Refresh the web page to see how the location changes showing the new exit point validating that the traffic is being redirected through Secure Access.
67)	Go back to the Secure Access web interface and in the left-hand menu, click Monitor and then click Activity Search and you should see data that shows what is coming from these branch networks into the Secure Access environment.
68)	End of Lab
