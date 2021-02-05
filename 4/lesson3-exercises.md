
# Exercise: Create a Single VM Blueprint

In this exercise, you will create a single VM blueprint and then download and launch the blueprint.

Creating a Single VM Blueprint
From the Prism Central tab in your browser, select the Entities menu > Services and click Calm.

Hover your mouse cursor over the icons at the far left of the browser to reveal the icon names. Click the Blueprints icon.

Click the + Create Blueprint drop-down menu and select Single VM Blueprint.

On the Create Single VM Blueprint page, use the following information to complete the fields:

Name: Single-VM-BP
Description: Single VM test for Calm
Project: HybridCloudEngineer
Click VM Details >. Verify the following fields are set as defined below:

Name: VM1
Cloud: Nutanix
Operating System: Linux
Directly below the Operating System field showing Linux, locate Clone from environment. Clicking this will clone/copy the VM configuration from the Environments section of the HybridCloudEngineer project to your VM configuration in this blueprint. The HybridCloudEngineer project VM configuration is the same as the Test-Project project you built in a previous exercise. Click Clone from environment.

The HybridCloudEngineer project environment VM configuration has now been copied to this blueprint. Click VM Configuration > and verify the VM settings using the following information:

vCPUs: 1
Cores per vCPU: 1
Memory (GiB): 2

Guest Customization Script field:

#cloud-config users:

name: @@{superuser.username}@@ ssh-authorized-keys:
@@{superuser.public_key}@@ sudo: ['ALL=(ALL) NOPASSWD:ALL']
Type: Disk
Bus Type: SCSI
Operation: Clone from Image Service
Image: CentOS-8-GenericCloud-8.2.2004-20200611.2.x86_64.qcow2
Bootable: Check box selected
NIC 1: default-net
Some settings are left to the administrators discretion as the settings may be unique to the deployment environment. Click the radio button next to Dynamic in the NIC 1 section for default-net.

Click Advanced Options at the bottom of the page. Scroll to the top of the display, click Add/Edit Credentials.

In the Credentials dialog box, click + Add Credentials. Use the following information to configure the new credential:

Credential Name: superuser
Username: centos
Secret Type: SSH Private Key
SSH Private Key: Click the arrow coming out of the box, navigate to C:\cygwin64\workspace.ssh\id_rsa and select Open. Ensure the inserted key text has the private key, -----BEGIN RSA PRIVATE KEY----- shown at the top.
Click Done.

Under CONNECTIONS, click the check box next to, Check log-in upon create and choose superuser from the Credentials pull-down menu.

Scroll to the bottom of the page and click Save.

Downloading and Launching the Blueprint
Select the Entities menu > Services and click Calm.

Hover your mouse cursor over the icons at the far left of the browser. Click the Blueprints icon, second from the top.

Click your Single-VM-BP blueprint then click Download at the upper right.

In the Download Blueprint dialog box, click the check box for Do you want to include credentials and secrets in the blueprint.

Type nutanix/4u in the Enter Passphrase box and click Continue to save the file.

Open Windows Explorer / File Manager on your Desktop task bar and open the Downloads folder. Copy the Single-VM-BP JSON file to the Workspace folder on the Windows Desktop to make it persistent.

Download all the files in your Workspace folder to your personal computer by right-clicking each file and choosing Download with Frame. This will save your files to your local systems default downloads folder. Be sure to get your SSH Keys in the .ssh folder.

From the Calm blueprint page, click Launch at the upper right. In the new view, you will see three tabs, with the Profile Configuration automatically selected. Type Single-VM-App in the Name of the Application field.

Click Create.

When the view changes and you see PROVISIONING, click Audit. Expand the provisioning view by clicking the turndown arrow to the left of Create. Continue to expand each new component as it appears, to follow the provisioning progress to completion.

Verify that Calm was able to log into the VM during the post-provisioning steps by confirming a Green circle around the Service1 - Check Login step, under the Service1 - Substrate Create tree.

Once the application is in a RUNNING state, click the Entities menu and go to Infrastructure > VMs. You will see your **Single-VM-BP” blueprint VM running.

Return to Entities > Services > Calm > Applications > Single-VM-App. Select the Manage tab.

Hover your cursor over the Stop row and click the arrow.

In the Run Action: Stop dialog box, click Run to confirm stopping the VM.

The Application page should now show STOPPING at the top.

When STOPPING changes to STOPPED, go back to Entities > Infrastructure > VMs and you should see the Single-VM-BP blueprint VM in a powered off state.

Exercise
In this exercise, you will:

Publish your single VM blueprint to the marketplace.
Launch your blueprint from the marketplace.
Unpublish and delete your blueprint.
We’ll walk through the steps together.

Publishing a Single VM Blueprint to the Marketplace
On the Prism Central tab in your browser, select the Entities menu, then Services and click Calm.

Select the Blueprints icon.

If Single-VM-BP is present, click on the blueprint and skip to step 8. If the blueprint is not present, this means you received a new cluster deployment and will need to upload your saved blueprint. Continue with the following steps.

Click Upload Blueprint and browse to the Workspace folder on your desktop and select your saved Single-VM-BP JSON file. Click Open.

In the Upload Blueprint dialog box, select HybridCloudEngineer in the Project pull-down menu and type nutanix/4u in the Passphrase field. Click Upload. This will place you into the Single-VM-BP blueprint you created and saved in a previous exercise.

Click VM Details and in the VM Details page, click VM Configuration.

From the VM Configuration page, locate the NICs section and under NIC1, Private IP, click the Dynamic radio button. Click Save.

On your Single-VM-BP blueprint page, click Publish in the upper right corner of the browser window.

In the Publish Blueprint dialog box, verify Publish as a, is set to New Marketplace Blueprint and type 1.0.0 in the Initial Version field.

The name should default to Single-VM-BPand the Description should default to Single VM test for Calm.

Click Submit for Approval.

Hover your cursor over the icons to the far left of the browser. Click Marketplace Manager icon (second icon from the bottom).

At the top, click the Approval Pending tab and click Single-VM-BP. A panel will appear on the right-side of the browser window.

Select HybridCloudEngineer in the Project Shared With drop-down menu.

Click the Approve button (box with a checkmark). The blueprint will be removed from the Approval Pending tab and placed in the Marketplace Blueprints tab.

At the top, click the Marketplace Blueprints tab. If you do not see your Single-VM-BP blueprint in the list, enter *single in the search field and press Enter. Select the checkbox next to the Single-VM-BP blueprint. Observe the current status of your blueprint, which should be Accepted**.

On the right-side of the browser window, verify HybridCloudEngineer is selected in the Projects Shared With drop-down menu and click Publish.

The blueprint Status column should now read Published.

Launching a Blueprint from the Marketplace
Select the Entities menu, then Services and click Calm.

Click the Marketplace icon in the left column.

Click Single-VM-BP blueprint and click Launch.

In the Launch dialog box, select the HybridCloudEngineer project if needed and click Launch if needed.

Type Single-MP-App (MP=Marketplace) in the Name of the Application field and click Create.

When the view changes and you see PROVISIONING, click Audit. Expand the provisioning view. Continue to expand each new component as it appears, to follow the provisioning progress to completion.

Verify that Calm can login to the VM during the post-provisioning steps by confirming a Green circle around the Check Login step, under the Service1 - Substrate Create tree.

Go to Entities > Infrastructure > VMs and verify your VMs. You should see the VM from the Single-VM-App application launched in an earlier exercise, in a powered off state and the VM launched with the Single-MP-App application, in a powered on state.

Deleting and Unpublishing a Blueprint
Go to Entities > Infrastructure > VMs and review the current VMs.

Return to Entities > Services and click Calm.

You should be on the Application page (default) and you should see the Single-VM-App and Single-MP-App applications listed.

Select the Single-MP-App check box.

Select Delete from the Action drop-down menu.

Click Confirm in the Delete Application dialog box.

You will be automatically switched to the Audit page. Monitor the delete process.

In the left column, click the Applications icon. Repeat steps 3-6 for Single-VM-App.

Click the Marketplace Manager icon on the left-side of your browser window (second from bottom) and enter single in the search field. Press Enter. Select the Single-VM-BP blueprint and in the right side panel, click Unpublish.

The Status of the Single-VM-BP will change to Accepted.

Click the trash can icon and click Delete in the Confirm Delete dialog box.

Click the Blueprints icon on the far left of the browser and click the check box next to Single-VM-BP blueprint.

Select Delete from the Action drop-down menu and click Delete in the Confirm Delete dialog box.
