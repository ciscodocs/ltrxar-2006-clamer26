# Visibility and ThousandEyes



Section 2 – Visibility & ThousandEyes
First section is to cover the basic visibility configurations under the basic system profile (I’ll fill that in shortly) 
Second section is the ThousandEyes Agent configuration and deployment.
Deploying the ThousandEyes Agent
It is critical in this section that as you begin – you have renamed your routers from the standard ‘brX-edgeY’ or ‘dcX-edgeY’ to put your pod number in front of the hostname:
 
The ThousandEyes agents will use this as their hostname and if you do not do it we will see the ThousandEyes system make up some unique identifiers to accommodate the hostnames that are not made unique by using the POD#.
Please note that in CML we’re using the Catalyst 8000v in SD-WAN mode. Technically the ThousandEyes agent is not supported on the Catalyst 8000v in SD-WAN mode. What this means is that we cannot use the automated workflows to do the installation process and we can’t use a standard ThousandEyes profile (created under ‘Other Profiles) which makes deployment of the Agent very simple. Instead, we will use a CLI profile which is still very simple but does require you know the CLI bits required that the ThousandEyes Automation workflow or the ThousandEyes Profile would do automatically. It’s a work around so we can show you these features in the lab in the CML environment. Please come speak with a proctor if you are interested in future compatibility of the ThousandEyes Agent and the Catalyst 8000v in SD-WAN mode.
The first thing that we need to do for this is to make sure that the ThousandEyes agent exists in the software repository on the SD-WAN Manager. 
Click on the Maintenance icon and then click on the Software Repository. Then click on the Virtual Images tab and validate that an x86 version of the ThousandEyes agent is on the system (should be a 5.1.2 and a 5.1.3). 
   
Next, navigate to the configuration groups. Then click on the CLI Add-on Profiles tab and then edit the branch_cli feature profile. This should be the profile you are currently using on all of your branch routers.
It should have one line configured in it at the moment:
platform console serial

**Add notes: platform resource service-plane-heavy

 
Add the following configuration to the CLI template.

```ios
iox
interface GigabitEthernet1
ip nat outside
interface VirtualPortGroup4
 ip address 192.0.2.5 255.255.255.252
 ip nat inside
ip nat inside source list 1 interface GigabitEthernet1 overload
ip access-list standard 1
 10 permit 192.0.2.0 0.0.0.3
app-hosting appid te
 app-vnic gateway0 virtualportgroup 4 guest-interface 0
  guest-ipaddress 192.0.2.6 netmask 255.255.255.252
 app-default-gateway 192.0.2.5 guest-interface 0
 app-resource docker
  prepend-pkg-opts
  run-opts 1 "-e TEAGENT_ACCOUNT_TOKEN=lqgjzffcoikgdgn3xytg62gwtx6mi9jw"
 name-server0 8.8.8.8
 start
```

This configuration is essentially what the workflow or the ThousandEyes Profile would deploy to the router. In fact, any other IOS-XE device (Cat9000 switches for example) would use the same configuration to host the ThousandEyes agent.
Notice the TEAGENT_ACCOUNT_TOKEN is “lqgjzffcoikgdgn3xytg62gwtx6mi9jw”, this connects to this lab’s instance of the ThousandEyes dashboard and once you deploy this new CLI Addon template to all 8 of your routers – please see the proctor to validate that your agents are communicating properly to the dashboard. 
  
 
You can see the agents on your Catalyst SD-WAN Manager dashboard as well via Monitor  Hosted Edge Services (should be at the bottom of the list)
 
If you click on the ‘te’ that’s highlighted blue, you can see more detail:
 
If you click on the three dots to the right of each agent, you can drill into the agent’s heath statistics:
 
And clicking ‘View device details’ will take you to the router’s health statistics.
 
Once you have deployed your agents to your routers reach out to your proctor so that they can deploy the test to your network. Once that is completed, you should see test results that look like this:
 
You can also validate the communication of the agents via the command ‘show app-hosting detail’
 
