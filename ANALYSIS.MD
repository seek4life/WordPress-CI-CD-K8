# *ANALYSIS.MD*


This application can be scaled by length and breadth in terms of volume and Size. Few enhancements that I can think of are:

1. We can scale this application horizontally by adding more pods. The database service uses MySQL, and this service can be scalled vertically.

2. Here MySQL database is used, If the application grows exponentially, and data volume is not able to sustain in MySQL then the databases like Cassandra or MongoDB can be used wherein the database tier can also be scaled vertically.

3. By having 2 k8 clusters we can achieve blue-green deployment which eases many operational tasks.

4. Email services can be leveraged to monitor the health of the cluster.

5. Data Visualization can be developed to report and understand application usage and plan for future growth.

6. Database service monitoring is needed to avoid any incidents.

7. The project can be integrated with Jira to plan the enhancements and other tasks.

8. Remote k8 cluster deployment can be done using Ansible.