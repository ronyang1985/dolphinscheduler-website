### Instructions for using the DolphinScheduler's Ambari plug-in

#### Note

1. This document is intended for users with a basic understanding of Ambari
2. This document is a description of adding the DolphinScheduler service to the installed Ambari service
3. This document is based on version 2.5.2 of Ambari 

#### Installation preparation

1. Prepare the RPM packages

   - It is generated by executing the command `mvn -U clean install -Prpmbuild -Dmaven.test.skip=true -X` in the project root directory (In the directory: dolphinscheduler-dist/target/rpm/apache-dolphinscheduler/RPMS/noarch)

2. Create an installation for DolphinScheduler with the user has read and write access to the installation directory (/opt/soft)

3. Install with rpm package

   - Manual installation (recommended）：
      - Copy the prepared RPM packages to each node of the cluster.
      - Execute with DolphinScheduler installation user: `rpm -ivh apache-dolphinscheduler-xxx.noarch.rpm`
      - Mysql-connector-java packaged using the default POM file will not be included.
      - The RPM package was packaged in the project with the installation path of /opt/soft. 
        If you use MySQL as the database, you need to add it manually.
      
   - Automatic installation with Ambari
      - Each node of the cluster needs to be configured the local yum source
      - Copy the prepared RPM packages to each node local yum source

4. Copy plug-in directory

   - copy directory ambari_plugin/common-services/DOLPHIN to ambari-server/resources/common-services/
   - copy directory ambari_plugin/statcks/DOLPHIN to ambari-server/resources/stacks/HDP/2.6/services/--stack version is selected based on the actual situation

5. Initializes the database information

   ```sql
   -- Create the database for the DolphinScheduler：dolphinscheduler
   CREATE DATABASE dolphinscheduler DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
   
   -- Initialize the user and password for the dolphinscheduler database and assign permissions
   -- Replace the {user} in the SQL statement below with the user of the dolphinscheduler database
   GRANT ALL PRIVILEGES ON dolphinscheduler.* TO '{user}'@'%' IDENTIFIED BY '{password}';
   GRANT ALL PRIVILEGES ON dolphinscheduler.* TO '{user}'@'localhost' IDENTIFIED BY '{password}';
   flush privileges;
   ```

#### Ambari Install DolphinScheduler
- **NOTE: You have to install Zookeeper first**

1. Install DolphinScheduler on Ambari web interface

   ![](/img/ambari-plugin/DS2_AMBARI_001.png)

2. Select the nodes for the DolphinScheduler's Master installation

   ![](/img/ambari-plugin/DS2_AMBARI_002.png)

3. Configure the DolphinScheduler's nodes for Worker, Api, Logger, Alert installation

   ![](/img/ambari-plugin/DS2_AMBARI_003.png)

4. Set the installation users of the DolphinScheduler service (created in step 1) and the user groups they belong to

   ![](/img/ambari-plugin/DS2_AMBARI_004.png)

5. System Env Optimization will export some system environment config. Modify according to the actual situation

   ![](/img/ambari-plugin/DS2_AMBARI_020.png)
   
6. Configure the database information (same as in the initialization database in step 1)

   ![](/img/ambari-plugin/DS2_AMBARI_005.png)

7. Configure additional information if needed

   ![](/img/ambari-plugin/DS2_AMBARI_006.png)

   ![](/img/ambari-plugin/DS2_AMBARI_007.png)

8. Perform the next steps as normal

   ![](/img/ambari-plugin/DS2_AMBARI_008.png)

9. The interface after successful installation

   ![](/img/ambari-plugin/DS2_AMBARI_009.png)
   
   

------



#### Add components to the node through Ambari -- for example, add a DolphinScheduler Worker

***NOTE***: DolphinScheduler Logger is the installation dependent component of DS Worker in Dolphin's Ambari installation (need to add installation first; Prevent the Job log on the corresponding Worker from being checked)

1. Locate the component node to add -- for example, node ark3

   ![DS2_AMBARI_011](/img/ambari-plugin/DS2_AMBARI_011.png)

2. Add components -- the drop-down list is all addable

   ![DS2_AMBARI_012](/img/ambari-plugin/DS2_AMBARI_012.png)

3. Confirm component addition

   ![DS2_AMBARI_013](/img/ambari-plugin/DS2_AMBARI_013.png)

4. After adding DolphinScheduler Worker and DolphinScheduler Logger components

   ![DS2_AMBARI_015](/img/ambari-plugin/DS2_AMBARI_015.png)

5. Start the component

   ![DS2_AMBARI_016](/img/ambari-plugin/DS2_AMBARI_016.png)


#### Remove the component from the node with Ambari

1. Stop the component in the corresponding node

   ![DS2_AMBARI_018](/img/ambari-plugin/DS2_AMBARI_018.png)

2. Remove components

   ![DS2_AMBARI_019](/img/ambari-plugin/DS2_AMBARI_019.png)
