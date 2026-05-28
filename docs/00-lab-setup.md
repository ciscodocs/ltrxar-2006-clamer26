# Lab Setup

## Task 1: Identify your Cisco Catalyst SD-WAN Manager URL

## Task 2: Import the Preconfigured Profiles

1. Download the ![lab start-up import file](https://github.com/CiscoLearning/ciscolive-ltrxar-2006/blob/main/sdwan_files/startup/export-2026-05-28T14_36_33.tar.gz) to your local desktop.

    !!! note
        You must **right-click** on the link above and click **Save** or **Save Link As** in your browser to download the configuration export. 

2. Login to the Cisco Catalyst SD-WAN Manager, using the provided URL for your pod and the credentials below:
    
    - Username: **admin**
    - Password: **C1sco12345!**

3. In Cisco Catalyst SD-WAN Manager, click **Configuration** then **Configuration Groups** to open the Configuration Group context.

    ??? hint
        <img src="../assets/lab01/task01/step-01_configuration-groups.png" alt="Configuration Groups" width="300px">

4. In the top navigation bar, click **Import** to open the file upload context.

    ??? tip "Step Help"
        <img src="../assets/images/setup/task02/step-04_import-group.png" alt="Import configuration group template link" width="300px">

5. In the file upload context, navigate to the directory where you saved the downloaded file from Step 1. Select the downloaded file, **`export-2026-05-28T14_36_33.tar.gz`**, and click **Open** to import the Configuration Group.
6. Verify the import was successful. If errors are encountered, notify your proctor for assistance.
