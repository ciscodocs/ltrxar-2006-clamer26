# Section Three – Security

1. Validate that the UTD image is present in the software repository of the vManage server.
2. Click the Maintenance icon on the left-hand menu and then click Software Repository.

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step02.png" width="300px" alt="Software Repository"/>

3. Click the Virtual Images Tab
4. Validate that “app-hosting_UTD-Snort-Feature-x86_64_1.0.1_SV3.1.81.0_XE17.15_secapp-utd.17.15.01a.1.0.1_SV3.1.81.0_XE17.15.x86_64.tar” exists. 

    ??? tip "Step Help"
        <img src="../assets/images/lab03/te_step04.png" width="900px" alt="Software Repository"/>

5. Open the Policy Groups configuration section by clicking Configuration Policy Groups.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step05.png" width="600px" alt="Software Repository"/>

6. In the top right, click Objects and Policies 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step06.png" width="300px" alt="Software Repository"/>

7. Click the Security profiles tab

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step07.png" width="300px" alt="Software Repository"/>

8. Click Intrusion Prevention, then click Add Intrusion Prevention 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step08.png" width="300px" alt="Software Repository"/>

9. Input the Intrusion Prevention Details 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step09.png" width="300px" alt="Software Repository"/>

    ```
       Profile name = IPS_InspectionPolicy_v01 
       Signature Set = Security 
       Inspection Mode = Protection
    ```
   
10. Click Save in the bottom right corner

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step10.png" width="200px" alt="Software Repository"/>

11. Click Advanced Inspection Profile 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step11.png" width="300px" alt="Software Repository"/>

12. Click Add Advanced Inspection Profile
13. Input the Advanced Inspection Profile Details 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step13.png" width="300px" alt="Software Repository"/>

    ```
        Profile Name = Adv_InspectionProfile_v01 
        Intrusion Prevention Pulldown = IPS_InspectionPolicy_v01 
    ```

14. Click Save in the bottom right corner 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step10.png" width="200px" alt="Software Repository"/>

15. Return to the Policy Group configuration page by clicking the  Policy Group above Objects and Profiles 
16. Click NGFW 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step16.png" width="600px" alt="Software Repository"/>

17. Click add NGFW Policy 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step17.png" width="600px" alt="step 17"/>

18. In the NGFW Policy workflow input the appropriate details:

    ```
       Policy name = NGFW_IPS_InspectionPolicy 
       Solution = SDWAN 
    ```
    
19. Click Next 
20. Select the Optional Configuration Group to associate with the NGFW Policy.
21. Select the branch configuration groups to add to the policy by checking the box next to them.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step23.png" width="900px" alt="step 23"/>

22. Click next
23. Edit Rule1 by clicking the pencil icon on the right side of the window and clicking edit from the drop down. 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step25.png" width="900px" alt="step 25"/>

24. Change the Action to Inspect 
25. Choose the Advanced Inspection Profile created in the previous section 
26. Check the Log Events box 
27. Click Save in the bottom right

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step29.png" width="300px" alt="step 27"/>

28. Click Next in the Create NGFW Policy window 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step30.png" width="600px" alt="step 28"/>

29. Click Create Policy, then click Exit Workflow
30. Go back to the Policy Groups configuration section by clicking Configuration, then Policy Groups 
31. Click Add Policy Group and input the appropriate details 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step31.png" width="300px" alt="create policy"/>

    ```
       Policy Group Name: NGFW_IPS_InspectionPolicyGroup 
       Solution – SDWAN 
    ```

32. Click Create

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step32.png" width="600px" alt="create"/>

33. In the new Policy Group, click on the NGFW drop down and select the previously created NGFW Policy.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step33.png" width="600px" alt="policy group"/>

34. Click Save 
35. Click +Add to associate the routers to the group. 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step35.png" width="900px" alt="associate"/>

36. Click Associate Devices, then click Next 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step36.png" width="900px" alt="Software Repository"/>

37. Select the boxes for the Br1, Br2 & Br3 routers and then click Next.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step37.png" width="900px" alt="click boxes"/>

38. Click Associate Devices 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step38.png" width="900px" alt="Associate Devices"/>

39. Click Provision Devices 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step39.png" width="600px" alt="Provision Devices"/>

40. Check the boxes for BR1, Br2 & Br3 and then click Next.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step40.png" width="900px" alt=click boxes"/>

41. Review that no configuration dialogs need to be filled in and then click Next 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step41.png" width="600px" alt="no configuration needed"/>

42. Preview the CLI to validate the new configuration 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step42.png" width="600px" alt="CLI Validate"/>

43. Click Deploy (UTD takes a few minutes to deploy)

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step43.png" width="600px" alt="deploy"/>

44. Click View Deployment Status to monitor the deployment of the policy 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step44a.png" width="600px" alt="deployment status"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step44b.png" width="600px" alt="Deployment status"/>

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step44c.png" width="600px" alt="Validation"/>

45. Log into the Cisco Modeling Labs web interface (admin/C1sco12345!)

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step45.png" width="600px" alt="Software Repository"/>

46. Click on the lab labeled sdwan to open the lab’s topology 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step46.png" width="600px" alt="Software Repository"/>

47. Right click on br1-vpn10-pc and click console to open the console. Do the same for br2-vpn-pc.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step47.png" width="600px" alt="Software Repository"/>

48. In the split screen below the lab, adjust the screen size to be usable by moving the dividing line in up with your mouse.
49. In each console tab, click ‘open console’ 
50. Log into the linux console, the username & password are admin & admin 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step50.png" width="600px" alt="Software Repository"/>

51. Run the 3 test IPS signatures 

    ```
    curl -A Brontok http://www.cisco.com
    curl -A Zeppelin http://www.cisco.com
    curl -A spam_bot http://www.cisco.com
    ```
    
52. Validate that each responds with the following message: “curl: (56) Recv failure: Connection reset by peer”

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step52.png" width="600px" alt="step"/>

53. In the Catalyst SD-WAN Manager, navigate to Monitor, Logs 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step53.png" width="300px" alt="step"/>

54. View the Alarms log and validate that the NGFW logged and blocked the connections based on the IPS rules.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step54.png" width="900px" alt="step"/>

