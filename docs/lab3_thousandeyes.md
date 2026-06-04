# Lab 3: ThousandEyes Agent Deployment

??? tip "Important Note"
    It is critical in this section that as you begin – you have renamed your router's hostnames from the standard ‘brX-edgeY’ or ‘dcX-edgeY’ to put your pod number in front of the hostname.
    The ThousandEyes agents will use this as their hostname and if you do not do it we will see the ThousandEyes system make up some unique identifiers to accommodate the hostnames that are not made unique by using the POD#.

??? tip "Important Note about Compatibility"
    Please note that in Cisco Modeling Labs we are using the Catalyst 8000v in SD-WAN mode. Technically the ThousandEyes agent is not supported on the Catalyst 8000v in SD-WAN mode. What this means is that we cannot use the automated workflows to do the installation process, and we can’t use a standard ThousandEyes profile (created under ‘Other Profiles) which makes deployment of the Agent very simple. Instead, we will use a CLI profile which is still very simple but does require you know the CLI bits required that the ThousandEyes Automation workflow or the ThousandEyes Profile would do automatically. It’s a work around so we can show you these features in the lab in the CML environment. Please come speak with a proctor if you are interested in future compatibility of the ThousandEyes Agent and the Catalyst 8000v in SD-WAN mode.

??? tip "Reference Link"
    <a href="https://www.cisco.com/c/en/us/support/docs/routers/catalyst-8000v-edge-software/220391-configure-thousand-eyes-enterprise-age.html#toc-hId-892662733 ">Configuration Reference for this section</a>

1. Verify the ThousandEyes Agent image is in the software repository on the server.
2. Click the Maintenance icon on the left hand menu and then click Software Repository.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step02.png" width="200px" alt="Software Repository"/>

3. Click the Virtual Images Tab
4. Validate that “app-hosting_ThousandEyes-Enterprise-Agent-x86_64_5.1.2_thousandeyes-enterprise-agent-x86_64-5.1.2.cisco.tar” exists.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step04.png" width="900px" alt="Verify Images"/>

5. Ensure that the names for each router that will have the ThousandEyes agent install on them have had their hostnames renamed to include the POD#
6. Use the ‘deployment method’ that’s been used through the lab, edit the system settings and input a new hostname based on the prescribed format.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step06.png" width="900px" alt="deploy"/>

7. Edit the CLI-Add Profile for each of the configuration groups. Click Configuration on the left hand menu, then click Configuration Groups
8. Select one of your configuration groups.
9. Click the pencil icon next to the CLI Add-on Profile for that Configuration Group

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step09.png" width="900px" alt="CLI-Add Profile"/>

10. In the Edit screen add the following lines of code:

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step10.png" width="900px" alt="Add CLI commands"/>

    ```ios
    platform resource service-plane-heavy
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

11. Click Save at the bottom of the CLI Add-on Profile window.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step11.png" width="300px" alt="Save button"/>

12. Click the #out of sync (depends on the number of routers assigned to the group) to step through the deployment process to send the new CLI Add-on profile to the routers. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step12.png" width="900px" alt="Add CLI commands"/>

13. During the Deployment process – check the Preview CLI to make sure the deployment is making the right changes. Click Deploy after reviewing the Preview is correct.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step13.png" width="900px" alt="Add CLI commands"/>

14. Click View Deployment Status to monitor the deployment for success. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step14a.png" width="300px" alt="Add CLI commands"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step14b.png" width="900px" alt="Add CLI commands"/>

15. Repeat this for each of the configuration groups you have configured (DC, Single Router Branch & Dual Router Branch). 
16. Once these tasks are complete – check with one of the lab proctors to see if your pod’s agents show up in the ThousandEyes Dashboard. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step16.png" width="900px" alt="Add CLI commands"/>

17. Check the status of the agents on the routers by clicking Monitor, Hosted Edge Services (should be at the bottom of the list)

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step17.png" width="900px" alt="Add CLI commands"/>

18. Click on the ‘te’ that’s highlighted blue, you can see more detail. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step18.png" width="900px" alt="Add CLI commands"/>

19. If you click on the three dots to the right of each agent, you can drill into the agent’s heath statistics. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step19.png" width="600px" alt="Add CLI commands"/>

20. You can also validate the communication of the agents via the CLI with the command ‘show app-hosting detail’ 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step20.png" width="900px" alt="Add CLI commands"/>

21. Once you have deployed your agents to your routers reach out to your proctor so that they can deploy the test to your network.
