# Private Cloud SaaS: Three-Tier Web Application Request

## Overview

After your previous success demonstrating manual provisioning of VMs on a private cloud, the Vice President of Software Engineering has set your next project for on-premises private cloud automation. In order to make the software developers productive, you are tasked to provide a rudimentary working model of The E-Commerce Company’s basic web site on the private cloud in order to save on public cloud costs for test and development workloads.

## Steps

To complete the project:

- Design a blueprint to deploy and configure a **three-tier web application with a load balancer, web server, and database VM** hosted on the private cloud.
- Insure each VM in configured with the proper resources and tasks.
- Test the deployment works with a read and write to the database.
- Test web tier scaling: scale-in and scale-out actions changing the population by a count of 1.
- Allow an end user to make a database backup.

## Requirements

The three tiers of the web application are:

1. load balancer,
2. web server,
3. database server.

Developers need a small deployment scenario, the ability to scale in and scale out the web tier, and to back up the database at any time.

### Private cloud VMs requirements

- Use the publicly available CentOS 8 image published on the CentOS website. The current image version's full URL is https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-GenericCloud-8.2.2004-20200611.2.x86_64.qcow2 - please copy and paste the full URL when creating image configurations.
- Create and use the ```teccadmin``` account to configure the OS and install software. Please see the notes below regarding credentials that are specific to database VMs.
- All VM accounts require using ***SSH access*** and can be granted ***sudo privileges***.

### Developer requirements for the application environment

- The load balancer VM will be a monolith for this project and use the latest HAProxy software included with the OS.
The web tier will use Apache web server and use the latest version of httpd software included with the OS.
- The web tier VMs should be named ```wwwX```, where X is a number. E.g: ```www0```, ```www1```, ```www2```, etc.
- The deployment scenario should be named small and each VM should use the same resource sizes: ***1 vCPU, 1 core, 1GB RAM***
- The developer should be able to manually scale the web tier in and out by one VM at a time and the load balancer must use the updated web tier population.
- After deployment, a developer can make a database backup at any time.
No VM resource parameters can be changed by the end user, except the Database VM memory.

### Database VMs requirements

- Be configured as a monolith for this project and use the latest version of MySQL, currently version 8.x.
- Make appropriate use of the teccdba Calm credential to ensure security standards are applied throughout the application lifecycle. This approach must be applied to both deployment and post-deployment operations.
Implement the ability to alter the root MySQL account at any time after the - MySQL database has been deployed, using a password that is provided at run-time.
- Have access to a database named webapp that is deployed with a single table named ```tecc_dba_test```.
- Be configured with a MySQL user named webapp_user that has all permissions on the webapp database and that is configured with the root password provided by the user.

### Software Release requirements for the environment to pass a test to ensure the configuration works for a developer

- The web app should be installed to the standard web docroot.
- Web app should use the database ```tecc_webuser``` user account and provided ***root password***
- Web app should read and write to the ```tecc_dba_test``` database table
- The web tier ***should scale between 2 and 4 VMs.***
- The test application and database schema source code are in:
[Github Repo](https://github.com/nutanixdev/udacity/tree/main/HybridCloudEngineer/course/2/project.) Specifically:
  - [SQL](https://raw.githubusercontent.com/nutanixdev/udacity/main/HybridCloudEngineer/course/2/project/tecc_dba_test.sql)
  - [PHP](https://raw.githubusercontent.com/nutanixdev/udacity/main/HybridCloudEngineer/course/2/project/tecc_dba_test.php)

***Please create, test, and publish the blueprint to satisfy these business requirements.***

## Submission

