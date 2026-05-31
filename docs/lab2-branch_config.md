# Lab 2: Build Branch Sites and Configure the Topology

## Task 1: Single Router Branches

1. Create a new **Configuration Group** named **branch_single_router_v01**.
2. Attach the following preconfigured profiles to the new Configuration Group:

    - System Profile: **default_system_v01**
    - Transport Profile: **srb_transport_v01**
    - Service Profile: **branch_corporate_vpn_v01**
    - CLI Add-On Profile: **default_cli_v01**

3. Add devices from branches **br1** and **br2** to the branch_single_router_v01 Configuration Group. When complete, devices **br1-edge1** and **br2-edge1** will be added to the configuration group.

    ??? tip "Step Help"
        <img src="../assets/images/lab02/task01/step-03_associate-devices.png" alt="Associate devices from br1 and br2" width="300px">

4. Download the ![CSV import file](https://github.com/CiscoLearning/ciscolive-ltrxar-2006/blob/main/sdwan_files/csv/single_router_branch-20260528.csv) to your local desktop.

    !!! note
        You must **right-click** on the link above and click **Save** or **Save Link As** in your browser to download the CSV import file.

5. Click **Deploy** for the **branch_single_router_v01** Configuration Group.
6. When prompted to Select Devices to Deploy, click **Global** to select the devices in branches br1 and br2, and then click **Next**.
7. On the "Add and Review Device Configuration" dialog, click **Global** to select all devices. The device selection context will appear.

    ??? tip "Step Help"
        <img src="../assets/images/lab02/task01/step-07_deploy-and-import.png" alt="Device selection context" width="300px">

8. Click **Import** to open the file selection dialog. Select the file you download in the previous step, and click **Open** or **Import**, depending on your browser, to import the CSV data. 

    ??? tip "Step Help"
        <img src="../assets/images/lab02/task01/step-08_import-link.png" alt="Import link location" width="300px">

9. If no errors are displayed, the import was successful. If you encounter errors during the import, notify your proctor.
10. Select the device **br1-edge1** and click **default_system_v01 (system)** to expand the System Profile.
11. Change the system hostname to **PODnn-br1-edge1**, where **nn** is your pod number for today's lab.

    ??? tip "Step Help"
        <img src="../assets/images/lab02/task01/step-10_br1-edge1.png" alt="Change the hostname of br1-edge1" width="300px">

12. Change the device to **br2-edge1** and set the system hostname to reflect your pod number for today's lab.

    ??? tip "Step Help"
        <img src="../assets/images/lab02/task01/step-11_br2-edge1.png" alt="Change the hostname of br2-edge1" width="300px">

13. Click **Next**, review the deployment details, then click **Deploy** to deploy the configuration to branches br1 and br2.
14. View the deployment status and verify all tasks completed successfully.


## Task 2: Dual Router Branch

Duplicate transport profile to drb_transport_v01
Add TLOC Extension interface
Create config group
- Same as previous, but with drb_transport_v01 profile.

Import .csv "dual_router_branch"

For each router, expand syste profile and change the hostname to PODxx-...

Deploy
Verify

## Task 3: Create Topologies

### Create Hub and Spoke Topology

### Create Full Mesh Topology

### Select the topology for the remainder of the lab