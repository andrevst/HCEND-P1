Hello CTO,

The Demo infrastructure is ready.

We had the NTP (pool.ntp.org) and the DNS ip 8.8.8.8 configured along with the production and development VM's.
Production VM's are at VNet 0 at the ip's 172.31.0.220 (application), 172.31.0.227 (webserver) and 172.31.0.228 (database). Also, these machines were cloned for development environment and updated to use development umanaged network. The Operational System images are at the Images Container Storage.

I made the scale out test at the DB VM and after create and test it I've added 1 more CPU and 2 more GB of ram so now the DB VM have 2 VCPU's and 6 GB of Ram, if needed we can scale in it to save resources.

Finally, the data protection domain prod-recovery was created and the VM db-prod was deleted and recovered as Nutanix-Clone-db-prod. The snapshot worked fine.
I believe that is it. You can find the evidences in the excel report attached. 

Please get in touch if you need anything else or have any questions.

Best Regards,

André
