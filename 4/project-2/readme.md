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
  - smallAHV: LAMP Application
  - MySQLAHV: MySQL 8.0 Database
- One application profile
  - Nutanix Application Profile
- VM and tasks follow security standards specified
  - Each user accountable for their part of the Application
  - Database users define as required
  - Database password validated by regex with the following rule:
    - Minimum eight characters, at least one uppercase letter, one lowercase letter, one number and one special character
    - Regex Used:```^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$```

### Web Application Configuration and Scaling Web Tier

> Enhance the blueprint to provide PaaS by configuring the web application and creating web-tier scaling

### Provide a Database Backup Action

> Achieve a SaaS-like experience for developers with delegated, post- deployment operations to back up the database
