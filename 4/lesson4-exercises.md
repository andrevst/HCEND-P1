# Lesson 4 - Exercises

## Create a Multi-VM Blueprint

The goal here is to build a load-balanced multi-VM web application using the LAMP stack. This is commonly referred to as a “two-tier” web application.

It uses a current CentOS distribution for the Linux OS and host the web application on the private cloud with Nutanix AHV as the VM hypervisor.

**First create a new multi-VM blueprint:**

- Add your SSH key credentials
- Add a WebServer VM and configure CentOS with cloud-init and shell tasks
- Install and configure an Apache web server
- Configure a one page PHP web application

**Then add a load balancer service:**

- Add an HAProxy VM and configure CentOS with cloud-init and shell tasks
- Install and configure HAProxy load balancer to serve requests to the WebServer

**Finnally, add scalability:**

- add scalability to the WebServer to increase application performance, making it a web-tier array

### Creating a Multi VM Blueprint

- From the Prism Central tab on your browser, select the Entities menu > Services and click Calm.
- Select the Blueprints icon.
- Click + Create Blueprint and select Multi VM/Pod Blueprint.
- Type WebApplication in the Name field and select your HybridCloudEngineer project.
- In the Description field, type: 2-Tier Web Application.
- Select Proceed. You will now be presented with the Calm Blueprint canvas.

#### Create Credentials

- Select Credentials above the canvas work area.
- Click + and use the information below to complete the dialog box:
  - Credential Name: superuser
  - Username: centos
  - Secret Type: SSH Private Key
  - SSH Private Key: In the upper right corner of the key field, click the box with the arrow, navigate to the C:\cygwin64\workspace.ssh\id_rsa private key and select Open.
- Click Save at the upper right and click Back.

*Note that the blueprint shows a red exclamation mark in the top of the blueprint canvas, this is expected at this early stage of building a new blueprint!*

#### Setup Configurations and Variables

- In the configuration panel on the left, which displays Application Profile Name, change the name from Default to Nutanix.
- Immediately below the Application Profile Name, click + next to Variables.
- For this variable, click the running man icon to toggle the color from grey to blue. This indicates a runtime variable that is changeable upon blueprint launch.
- Add the following variable fields, please note that uppercase value of USER_INITIALS is important and that you should change the value of xyz to your initials or any name you prefer.
  - Name: USER_INITIALS
  - Data Type: String
  - Value: xyz
  - Secret: Do not change; keep unchecked.
- Click Save at the top of the blueprint canvas and then click Configuration.

*Note that the blueprint still shows a red exclamation mark - this is expected!*

#### Configure Image

- Go to configuration then click + next to Downloadable Image Configuration (0) to add a URL for a network accessible image.
- Use the values below to complete the form. You can open C:\Scripts\CentOS8_URL.txt in a text editor, then copy and paste the contents for Source URL, below.
  - Package Name: CentOS_8_Cloud
  - Description Image: CentOS 8 Cloud Image
  - Image Name: CentOS_8_Cloud
  - Image Type: Disk Image
  - Architecture: X86_64
  - Source URI: [https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-GenericCloud-8.2.2004-20200611.2.x86_64.qcow2](https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-GenericCloud-8.2.2004-20200611.2.x86_64.qcow2)
  - Product Name: CentOS
  - Product Version: 8
- Click Save at the upper right and then click Back.

*Note that the blueprint still shows a red exclamation mark - this is expected!*

***Click the Blueprints icon in the left column. Check the box next to your blueprint and from the Action menu, select Download to save your blueprint to your workstation. Check the box for Downloading credentials and type the passphrase nutanix/4u.***

### Create Service

With the general settings in the blueprint have been configured and saved, add a new service to the blueprint to support web server configurations. 

- From the Blueprint page, click your WebApplication blueprint.
- Select + next to Service in the Application Overview window at the bottom left.
- A new service will be created in the blueprint canvas and at the right a service configuration pane will populate information for the new Service1 service. Configure the following fields:
  -Service Name: Change Service1 to WebServer
  -Name: Change VM1 to WebServerAHV
  -Cloud: Nutanix
  -Operating System: Linux
-Immediately below these settings is the Clone from environment option. *This option will populate the VM configuration based on the VM settings in the Calm Project selected for this blueprint.*
- Click Clone from environment and use the following to verify the settings.
  - VM Name: WebServer-@@{calm_array_index}@@-@@{calm_time}@@
  - vCPUs: 1
  - Cores per vCPU: 1
  - Memory (GiB): 2
  - Guest Customization: Check box selected
  - Type: Cloud-init
  - Script: Cloud-init configuration. If empty, select the Upload icon in the upper right hand corner of the cloud-init text box and browse to: C:\scripts\cloud-init.txt.
  - Device Type: Disk
  - Device Bus: SCSI
  - Operation: Clone from Image Service
  - Image: CentOS_8_Cloud
  - Bootable: Check box selected
  - NETWORK ADAPTERS (NICS): NIC 1 shows default-net.
- Under NETWORK ADAPTERS (NICS), next to Private IP, click the radio button next to Dynamic.
- Under CONNECTION, ensure the check box next to Check log-in upon create is selected. For the Credentials menu, select superuser.
- Click Save at the top of the page: **the red exclamation mark should disappear.** 
- **If the exclamation mark remains, click on the exclamation mark to see what remediation steps are required. Make corrections and save your blueprint, repeat until the exclamation mark disappears.**

***Select Download to save your blueprint to your workstation. Check the box for Downloading credentials and type the passphrase nutanix/4u.***

- On the left side, for the WebServer service, click on the turn-down arrow to reveal the service actions. Note that you can toggle the turn-down arrow to reveal or hide service actions.
- Choose the Restart service action. Note that the service on the blueprint canvas changes to display the Restart action, which is empty.

### Package Configuration

- On the blueprint canvas, click on the “WebServer” service white box to display the VM Service Configuration panel on the right and select the Package tab next to VM.
- Type WebServer_Package under Package Name.
- Click Configure Install.
- In the blueprint canvas, you will see Package Install appear within the WebServer service you have created.
- Click + Task and configure the following in the right-hand Configuration panel.
  - Task Name: Linux_OS_Update
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the Linux_OS_Update.txt file and click Open. This will upload the script to the Script box.
- Below the Script field, click Publish to Library.
  - In the Publish Task dialog box, note the saved task name and verify your install task configuration and click Publish at the lower right. This saves this task so it can be used in other blueprints or other services in the same blueprint.

***If you previously saved this task to the library, provide just the Task Name and Type as specified below, then click the Browse Library button to source the Linux_OS_Update task and save a few steps. If not found, provide all of the task configuration:***

### Create WebServer Tasks and Actions

#### Creating WebServer Restart Task

- On your blueprint canvas, select + Task to add a task in the WebServer service Restart service action and fill out the following fields in the configuration panel:
  - Task Name: WebServer_Restart
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the WebServer_ServiceAction_Restart.txt file and click Open. This will upload the script to the Script box.

#### Creating WebServer Start Action

- On the left side, for the WebServer service, choose the Start service action. Note that the service on the blueprint canvas changes to display the Start action, which is empty.
- On your blueprint canvas, select + Action to add a Start service action in the WebServer service and fill out the following fields in the configuration panel:
  - Task Name: Leave blank; this will be automatically populated.
  - Service Actions: Choose the Restart action.

***Note: This reuses the existing Restart service action, because for the Apache web server, these service actions are effectively the same operation.***

#### Creating WebServer Stop 

- On the left side, for the WebServer service, choose the Stop service action. Note that the service on the blueprint canvas changes to display the Stop action, which is empty.
- On your blueprint canvas, select + Task to add a Stop service action in the WebServer service and fill out the following fields in the configuration panel:
  - Task Name: WebServer_Stop
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the WebServer_ServiceAction_Stop.txt file and click Open. This will upload the script to the Script box.
  - Save it

#### Create Install WebServer Task

- Click + Task to add a second task and configure the following in the right-hand Configuration panel:
  - Task Name: Install_WebServer
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the WebServer_Install.txt file and click Open. This will upload the script to the Script box.
  - Save it

#### Create 2 Tier Web App Task

Click + Task to add a third task and configure the following in the right-hand Configuration panel:

Task Name: 2Tier_Webapp
Type: Execute
Script Type: Shell
Endpoint (Optional): Skip; leave blank
Credential: superuser
Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the WebServer_2Tier_Webapp.txt file and click Open. This will upload the script to the Script box.
Note: For this lesson, we are using a two tier web application. In the next lesson, we will update the web application to use the database, so please DO NOT USE the WebServer_3Tier_Webapp.txt by accident!

### Configure Uninstall Task

Click WebServer in the WebServerAHV service icon in the blueprint canvas and select the Package tab in the Configuration Panel.

Click Configure uninstall.

On your blueprint canvas, select + Task to add another task in the WebServer service and fill out the following fields in the configuration panel:

Task Name: Uninstall_WebServer
Type: Execute
Script Type: Shell
Credential: superuser
Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the WebServer_Uninstall.txt file and click Open. This will upload the script to the Script box.
Click WebServer in the WebServerAHV service icon in the blueprint canvas and select the Service tab in the Configuration Panel.

Change the Number of Replicas to reflect the following values:

Default: 2
Min: 2
Max: 4
Click Save

Select Download to save your blueprint to your Frame workstation. Check the box for Downloading credentials and type the passphrase nutanix/4u.

Click Continue and save the file. Go to the Downloads folder and copy the file to your Workspace folder. Save only the most recent copy!

Note: You can also download the file to your personal computer by right-clicking the file and choosing Download from Frame. This will save the file to your local systems default downloads folder.

## HAProxy Configuration

Now let’s walk through the steps to configure a load balancer to depend on a web server service in a two-tier web application.

Select the Entities menu > Services and click Calm.

Select the Blueprints icon in the left column.

If WebApplication is present, click on the blueprint and skip to step 7. If the blueprint is not present, this means you received a new cluster deployment and will need to upload your saved blueprint. Continue with the following steps.

Click Upload Blueprint and browse to the Workspace folder on your desktop and select your saved WebApplication JSON file. Click Open.

In the Upload Blueprint dialog box, select HybridCloudEngineer in the Project pull-down menu and type nutanix/4u in the Passphrase field. Click Upload. This will place you into the WebApplication blueprint you created and saved in previous exercises.

On the blueprint canvas, click WebServer in the WebServerAHV service and then the VM tab in the right configuration panel, scroll down to NETWORK ADAPTERS (NICS) and click + to add a NIC:

Under NIC 1 click the dropdown and select default-net.
Select Dynamic next to Private IP.
Click Save.

### Create a new Service

- Select ```+``` next to Service in the Application Overview window on the bottom left of the blueprint canvas
- Select the VM tab and enter in the following information:
  - Service Name: HAProxy
  - Name: HAProxyAHV
  - Cloud: Nutanix
  - Operating System: Linux
- Click Clone from environment to populate the VM configuration from the Project. Use the information below to verify your settings.
  - VM Name: ```HAProxy-@@{calm_version}@@-@@{calm-time}```
  - vCPUs: 2
  - Cores per vCPU: 1
  - Memory (GiB): 2
  - Guest Customization: Check box selected
  - Type: Cloud-init
  - Script: CloudInit script is shown.
  - Device Type: Disk
  - Device Bus: SCSI
  - Operation: Clone from Image Service
  - Image: CentOS_8_Cloud
  - Bootable: Select check box
  - NETWORK ADAPTERS (NICS): NIC 1 has default-net selected.
- Under NETWORK ADAPTERS (NICS), next to Private IP, click the radio button next to Dynamic.
- Under CONNECTION, ensure the check box next to Check log-in upon create is selected. For the Credentials menu, select superuser.
- Click Save at the top of the page.

***If any errors or warnings exist, hover your cursor over the red exclamation mark to see what remediation steps are required.***

### Create Sevice Actions

On the left side, for the HAProxy service, click on the turn-down arrow to reveal the service actions.

#### Create Restart Action

- Choose the Restart service action. Note that the service on the blueprint canvas changes to display the Restart action, which is empty.
- On your blueprint canvas, select + Task to add a task in the HAProxy service Restart service action and fill out the following fields in the configuration panel:
  - Task Name: HAProxy_Restart
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the HAProxy_ServiceAction_Restart.txt file and click Open. This will upload the script to the Script box.
- Save it

### Start Action
  
- On the left side, for the HAProxy service, choose the Start service action.
- On your blueprint canvas, select + Action to add a Start service action in the HAProxy service and fill out the following fields in the configuration panel:
  - Task Name: Leave blank; this will be automatically populated.
  - Service Actions: Choose the Restart action.
  - Note: This reuses the existing Restart service action, because for the HAProxy Load Balancer, these service actions are effectively the same operation.

### Stop Service

- On the left side, for the HAProxy service, choose the Stop service action.
- On your blueprint canvas, select + Task to add a Stop service action in the HAProxy service and fill out the following fields in the configuration panel:
  - Task Name: HAProxy_Stop
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the HAProxy_ServiceAction_Stop.txt file and click Open. This will upload the script to the Script box.
- Save it

### Package Configuration for HAProxy

- Scroll to the top of the configuration panel on the right and select Package next to VM.
Type HAProxy_Package

#### Configuring Install

- Under Package Name and click Configure Install.
- In the blueprint canvas, you will see Package Install appear within the HAProxyAHV service.
- Click the Click + Task and configure the following in the right-hand Configuration panel.

#### Adding OS Update

- If you previously saved Linux_OS_Update task to the library, provide just the Task Name and Type as specified below, then click the Browse Library button to source the Linux_OS_Update task and save a few steps. If not found, provide all of the task configuration:
  - Task Name: Linux_OS_Update
  - Type: Execute
  - Script Type: Shell
  - Endpoint (Optional): Skip; leave blank
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the Linux_OS_Update.txt file and click Open. This will upload the script to the Script box.

##### Adding Install Task for HAProxy

- Select + Task to add a second task and configure the following in the right-hand configuration panel:
  - Task Name: HAProxy_Install
  - Type: Execute
  - Script Type: Shell
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the HAProxy_Install.txt file and click Open. This will upload the script to the Script box.

##### Configuring HAProxy Uninstall

Select HAProxy within the HAProxyAHV service in the blueprint canvas and select the Package tab in the Configuration Panel.

Click Configure uninstall.

Select + Task and fill out the following fields in the configuration panel:

Task Name: Uninstall_HAProxy
Type: Execute
Script Type: Shell
Credential: superuser
Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the HAProxy_Uninstall.txt file and click Open. This will upload the script to the Script box.
Select Save.

Download the blueprint. Check the box for Downloading credentials and type the passphrase nutanix/4u.

## Create Web Server Scale

Finally we’ll walk through web server replicas and scale-in and scale-out operations.

***If WebApplication is present move next.***

- If the blueprint is not present, this means you received a new cluster deployment and will need to upload your saved blueprint. Continue with the following steps.
- Upload the Blueprint
- Under NICS for each Service:
  - Under NIC 1 click the dropdown and select *default-net*.
  - Select *Dynamic* next to *Private IP*.
  - Click Save.

### Adding Scale Out to Application

- At the lower left of the blueprint canvas, expand the Application Profiles box.
- Under Application Profiles, click the + next to Actions, under Nutanix. *You should see three things happen, a new Action# will be added to the Actions list, Action boxes are added to the bottom of all services on the blueprint canvas, and the Action configuration panel will be presented at the right.*
- In the configuration panel, change the Action Name to WebScaleOut. Notice the Action boxes below the services get a name change to WebScaleout.
- Click + Task in the lower WebScaleOut box for the WebServer AHV service. **Do not click on the upper box of the WebServer service, which has both + Task and + Action.**
- In the right configuration panel, change Task1 to ScaleOut in the Task Name field.
- In the Scaling Type menu, select Scale Out. In the Scaling Count field, type 1.
- Click Save at the top of the page.

### Updating HAProxy when Scaling out

**The HAProxy service configuration needs to be updated when scaling out the web servers.**

#### Creating Scale Update Task for HAproxy

- Under Applications Profiles, click WebScaleOut.
- On the blueprint canvas, in the HAProxy box, click the upper + Task next to +Action.
- In the new configuration panel at the right, fill out the following fields:
  - Task Name: HAProxy_Update
  - Type: Execute
  - Script Type: Shell
  - Credential: superuser
  - Script: In the upper right corner of the Script box, hover your mouse over the icons and click Upload script. Navigate to C:\Scripts. Select the HAProxy_Update.txt file and click Open. This will upload the script to the Script box. Click Save.

#### Creating a Scale Action for HAProxy

- In the HAProxy service box, click + Action.
- In the new configuration panel at the right, click the Service Actions menu and select Restart. The Task Name should change to Restart. This restart action is added to the HAProxy service and is automatically linked to the HAProxy_Update task.

#### Creating a task dependency between the WebScaleOut, ScaleOut task and the HAProxy_Update task

- Click the ScaleOut task in the bottom box, under the WebServer service and immediately to the right, click the curved arrow next to the trash can. Now click the HAProxy_Update task.
- A dependency link will be created.
- Click Save.

### Addind Scale In Application

- Under Application Profiles, click the + next to Actions once again. ***A new Action# will be added to the Actions list, new Action boxes are added to the bottom of all services on the blueprint canvas and a new Action configuration panel will be presented at the right.***
- In the configuration panel, change the Action Name to WebScaleIn. Notice the Action boxes below the services get a name change to WebScaleIn.
- Click +Task in the lower WebScaleIn box for the WebServer AHV service.
- In the right configuration panel, change Task1 to ScaleIn in the Task Name field.
- In the Scaling Type menu, select Scale In. In the Scaling Count field, type 1.
- Click Save at the top of the page.

### Updating HAProxy when Scaling In

**The HAProxy service configuration needs to be updated when scaling in the web servers.**

- **For WebScaleIn, to the same config for [HAProxy Task](#### Creating Scale Update Task for HAproxy) and for
[HAproxy Action](#### Creating Scale Update Action for HAproxy)**

#### Creating a task dependency between the WebScaleIn, ScaleIn task and the HAProxy_Update task

- Click the ScaleInt task in the bottom box, under the WebServer service and immediately to the right, click the curved arrow next to the trash can. Now click the HAProxy_Update task.
- A dependency link will be created.
- Click Save.

***Download the blueprint. Check the box for Downloading credentials and type the passphrase nutanix/4u.***
