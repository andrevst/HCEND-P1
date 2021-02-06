# Private Cloud SaaS: Three-Tier Web Application

This project deliveries a Nutanix Calm Blueprint in JSON to delivery the [project 2 requests](project2-request.md). It consist in a 3 Tier LAMP application hosted on private cloud.

## Project Deliveries

### Create Web App Blueprint Satisfying Business Requirements

>Create a three-tier web application blueprint that satisfies the IaaS business requirements for self-service private cloud deployment and security scenarios

- Two SSH credentials
  - teccadmin: Manages Loadbalancer and webapp VMs
  - teccdba: Manages MySQL VMs
- Three VM service tiers
  - HAProxyAHV: Load Balancer
  - WebServerAHV: LAMP Application
  - MySQLAHV: MySQL 8.0 Database
- One application profile
  - small Application Profile: ```deployment scenario === application profile```
- VM and tasks follow security standards specified
  - Each user accountable for their part of the Application
  - Database users define as required
  - Database password validated by regex with the following rule:
    - Minimum eight characters, at least one uppercase letter, one lowercase letter, one number and one special character
    - Regex Used:```^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$```

### Web Application Configuration and Scaling Web Tier

> Enhance the blueprint to provide PaaS by configuring the web application and creating web-tier scaling

- Submitted a blueprint with three service VM configuration of:

Load balancer server software
Web tier server software
Database server software
Submitted blueprint has:

a web application, configured to read and write to the database
post-deployment operation: a web-tier that scales and orchestrate updates to the load balancer configuration
web tier scaling actions that update the load balancer configuration

### Provide a Database Backup Action

> Achieve a SaaS-like experience for developers with delegated, post- deployment operations to back up the database

- Submitted a blueprint with: a database backup custom action, which uses the root account with end-user provided password.

Make the web tier scale-in and scale-out actions driven by a variable/macro provided by the user instead of hard coding it to a static value 1.
Make the database backup action orchestrate the stop and (re)start of web tier or HAProxy services before and after backup for the proper order of operations.
Make cloud-init leverage macros/variables instead of hard coding account names and public keys.
Make the database password a mandatory field, use regular expressions to validate 8 or more characters.