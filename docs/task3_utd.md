# Section Three – Security

## Next Generation Firewall
1. Validate that the UTD image is present in the software repository of the vManage server:
2. Click the Maintenance icon on the left-hand menu and then click Software Repository. 
3. Click the Virtual Images Tab 
4. Validate that “app-hosting_UTD-Snort-Feature-x86_64_1.0.1_SV3.1.81.0_XE17.15_secapp-utd.17.15.01a.1.0.1_SV3.1.81.0_XE17.15.x86_64.tar” exists. 
5. Open the Policy Groups configuration section by clicking Configuration Policy Groups. 
6. In the top right, click Objects and Policies 
7. Click the Security profiles tab 
8. Click Intrusion Prevention, then click Add Intrusion Prevention 
9. Input the Intrusion Prevention Details 
10. Profile name = IPS_InspectionPolicy_v01 
11. Signature Set = Security 
12. Inspection Mode = Protection 
13. Click Save in the bottom right corner 
14. Click Advanced Inspection Profile 
15. Click Add Advanced Inspection Profile 
16. Input the Advanced Inspection Profile Details 
17. Profile Name = Adv_InspectionProfile_v01 
18. Intrusion Prevention Pulldown = IPS_InspectionPolicy_v01 
19. Click Save in the bottom right corner 
20. Return to the Policy Group configuration page by clicking the  Policy Group above Objects and Profiles 
21. Click NGFW 
22. Click add NGFW Policy 
23. If you get the ‘Create NGFW Policy’ window, check the ‘don’t show this to me again’ box on the left of the window and then click Let’s do it on the right of the window. 
24. In the NGFW Policy workflow input the appropriate details:
25. Policy name = NGFW_IPS_InspectionPolicy 
26. Solution = SDWAN 
27. Click Next 
28. Select the Optional Configuration Group to associate with the NGFW Policy 
29. Select the branch configuration groups to add to the policy by checking the box next to them 
30. Click next
31. Edit Rule1 by clicking the pencil icon on the right side of the window and clicking edit from the drop down. 
32. Change the Action to Inspect 
33. Choos the Advanced Inspection Profile created in the previous section 
34. Check the Log Events box 
35. Click Save in the bottom right
36. Click Next in the Create NGFW Policy window 
37. Click Create Policy, then click Exit Workflow 
38. Go back to the Policy Groups configuration section by clicking Configuration, then Policy Groups 
39. Click Add Policy Group and input the appropriate details 
40. Policy Group Name: NGFW_IPS_InspectionPolicyGroup 
41. Solution – SDWAN 
42. Click Create 
43. In the new Policy Group, click on the NGFW drop down and select the previously created NGFW Policy. 
44. Click Save 
45. Click +Add to associate the routers to the group. 
46. Click Associate Devices, then click Next 
47. Select the boxes for the Br1, Br2 & Br3 routers and then click Next. 
48. Click Associate Devices 
49. Click Provision Devices 
50. Check the boxes for BR1, Br2 & Br3 and then click Next. 
51. Review that no configuration dialogs need to be filled in and then click Next 
52. Preview the CLI to validate the new configuration 
53. Click Deploy (UTD takes a few minutes to deploy)
54. Click View Deployment Status to monitor the deployment of the policy 
55. Log into the Cisco Modeling Labs web interface (admin/C1sco12345!)
56. Click on the lab labeled sdwan to open the lab’s topology 
57. Right click on br1-vpn10-pc and click console to open the console. Do the same for br2-vpn-pc. 
58. In the split screen below the lab, adjust the screen size to be usable by moving the dividing line in up with your mouse.
59. In each console tab, click ‘open console’ 
60. Log into the linux console, the username & password are admin & admin 
61. Run the 3 test IPS signatures 
62. curl -A Brontok http://www.cisco.com
63. curl -A Zeppelin http://www.cisco.com
64. curl -A spam_bot http://www.cisco.com
65. Validate that each responds with the following message: “curl: (56) Recv failure: Connection reset by peer” 
66. In the Catalyst SD-WAN Manager, navigate to Monitor, Logs 
67. View the Alarms log and validate that the NGFW logged and blocked the connections based on the IPS rules. 
68. End of NGFW section
