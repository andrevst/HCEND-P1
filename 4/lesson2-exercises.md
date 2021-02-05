# Exercises

1. Create a Calm project
2. Use the project you created to publish a default blueprint to the Calm Marketplace.
3. Launch the blueprint from the marketplace, name the application and audit the deployment process.
4. Delete the application.

## 1. Setup  a Calm Project

### a. From the Prism Central  go to Services -> click Calm

### b. Slect Project Tab then click  ```+ Create Project``` button and use the values below to complete the General Settings section

- Project Name: Test-Project
- Description: Test workload

#### b.i To the right of Infrastructure, click Select Provider and select Nutanix:

- The pre-selected account should be: NTNX_LOCAL_AZ

#### b.ii Click Select Clusters & Subnets.

- In the Select Subnets dialog box, select your cluster.
- Click the check box next to default-net and click Confirm.
- Use the values below to set Quotas:
  - vCPUs: 20
  - Storage: 1000
  - Memory: 48
- Click Save & Configure Environment.

*A message showing Project data successfully accepted by server briefly appears.*

#### b.iii Switch to the Environment page

> The Environment page allows you to define the components necessary to deploy a VM.

- On the Environment page, the Linux configuration should already be expanded.
- Use the values below to configure the VM
  - vCPUs: 1
  - Cores per vCPU: 1
  - Memory (GiB): 2
  - Click the check box next to Guest Customization and add:

  ```shell
  #cloud-config
      users:
        - name: centos
      ssh-authorized-keys:
      - @@{superuser.public_key}@@
      sudo: ['ALL=(ALL) NOPASSWD:ALL']
  ```

- Expand the DISK (1) section and use the configuration:
  - Type: Disk
  - Bus Type: SCSI
  - Operation: Clone from Image
  - Image: Select the CentOS8 image
  - Bootable: Select the check box

Scroll down to NETWORK ADAPTERS (NICS)

- add a NIC and configure the NIC:
  - NIC 1: default-net
  - Private IP: Select Dynamic

Scroll down to Connection.

- Credential drop-down menu and select Add New Credential with the following configuration:
  - Credential Name: superuser
  - Username: centos
  - Secret Type: SSH Private Key
  - SSH Private Key: In the upper right corner of the key field, click the box with the arrow, navigate to: ```C:\cygwin64\workspace.ssh\id_rsa``` and select Open. Ensure the inserted key text has ```-----BEGIN RSA PRIVATE KEY-----``` shown at the top. *Note: Adding new credentials can also be done from the top of the page by clicking the + next to Credentials.*
  - Ensure the box next to Check Log-in upon create is selected.

## 2 Exercise - Publishing a Blueprint to the Marketplace

Select the Entities menu (the three-lined hamburger icon) > Services and click Calm.

Hover your mouse cursor over the icons to the far left of the browser. Click the icon named Marketplace Manager. On the Marketplace Manager page, under the Marketplace Blueprint tab, you will find a default blueprint called ExpressLaunch.

Click the check box next to ExpressLaunch (default blueprint) in the Marketplace Blueprints view.

In the ExpressLaunch panel at the far right of your browser, remove the default project in the Projects Shared With drop-down menu. Select Test-Project and click Apply.

In the ExpressLaunch panel, click Publish and verify that the Status column for the blueprint shows Published.

Hover your mouse cursor over the icons to the far left of the browser. Click the Marketplace icon. You should see ExpressLaunch listed.

## 3 Exercise: Launching a Blueprint from the Marketplace

Select the Entities menu > Services and click Calm.

Click the Marketplace icon in the left column.

Click the ExpressLaunch blueprint and click Launch.

Select Test-Project.

In the Name of the Application field, type Test-App and click Create.

When the view changes and you see PROVISIONING, click Audit. Expand the provisioning view. Continue to expand each new component as it appears to follow the provisioning progress to completion.

## 4. Exercise: Deleting an Application

Select the Entities menu > Services and click Calm.

Hover your mouse cursor over the icons at the far left of the browser. Click the Applications icon.

Select the checkbox next to Test-App.

Select Delete from the Action drop-down menu.

Click Confirm in the Delete Application dialog box.

Click Audit to monitor the Delete process.

NOTE: Make sure to delete applications when directed to do so. If left running, you will run out of cluster resources and will be unable to launch applications in future exercises.


