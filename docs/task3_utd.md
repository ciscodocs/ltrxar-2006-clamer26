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
        <img src="../assets/images/lab04/utd_step17.png" width="600px" alt="Software Repository"/>

18. In the NGFW Policy workflow input the appropriate details:

    ```
       Policy name = NGFW_IPS_InspectionPolicy 
       Solution = SDWAN 
    ```
    
21. Click Next 
22. Select the Optional Configuration Group to associate with the NGFW Policy.
23. Select the branch configuration groups to add to the policy by checking the box next to them.

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step23.png" width="900px" alt="Software Repository"/>

24. Click next
25. Edit Rule1 by clicking the pencil icon on the right side of the window and clicking edit from the drop down. 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step25.png" width="900px" alt="Software Repository"/>

26. Change the Action to Inspect 
27. Choose the Advanced Inspection Profile created in the previous section 
28. Check the Log Events box 
29. Click Save in the bottom right

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step29.png" width="900px" alt="Software Repository"/>

30. Click Next in the Create NGFW Policy window 

    ??? tip "Step Help"
        <img src="../assets/images/lab04/utd_step30.png" width="900px" alt="Software Repository"/>

31. Click Create Policy, then click Exit Workflow 
32. Go back to the Policy Groups configuration section by clicking Configuration, then Policy Groups 
33. Click Add Policy Group and input the appropriate details 
34. Policy Group Name: NGFW_IPS_InspectionPolicyGroup 
35. Solution – SDWAN 
36. Click Create 
37. In the new Policy Group, click on the NGFW drop down and select the previously created NGFW Policy. 
38. Click Save 
39. Click +Add to associate the routers to the group. 
40. Click Associate Devices, then click Next 
41. Select the boxes for the Br1, Br2 & Br3 routers and then click Next. 
42. Click Associate Devices 
43. Click Provision Devices 
44. Check the boxes for BR1, Br2 & Br3 and then click Next. 
45. Review that no configuration dialogs need to be filled in and then click Next 
46. Preview the CLI to validate the new configuration 
47. Click Deploy (UTD takes a few minutes to deploy)
48. Click View Deployment Status to monitor the deployment of the policy 
49. Log into the Cisco Modeling Labs web interface (admin/C1sco12345!)
50. Click on the lab labeled sdwan to open the lab’s topology 
51. Right click on br1-vpn10-pc and click console to open the console. Do the same for br2-vpn-pc. 
52. In the split screen below the lab, adjust the screen size to be usable by moving the dividing line in up with your mouse.
53. In each console tab, click ‘open console’ 
54. Log into the linux console, the username & password are admin & admin 
55. Run the 3 test IPS signatures 
56. curl -A Brontok http://www.cisco.com
57. curl -A Zeppelin http://www.cisco.com
58. curl -A spam_bot http://www.cisco.com
59. Validate that each responds with the following message: “curl: (56) Recv failure: Connection reset by peer” 
60. In the Catalyst SD-WAN Manager, navigate to Monitor, Logs 
61. View the Alarms log and validate that the NGFW logged and blocked the connections based on the IPS rules. 
62. End of NGFW section
