# Migration_of_mysqlDB_from_onpremserver_to_awscloud_db_using_aws_dms

Migration of MYSQL DataBase to Cloud AWS using AWS DMS(DATA MIGRATION SERVICE)

Step:1.Setting up Onpremises Server(ubuntu server) where MySQL server will be installed and accessed to create,manipulate etc the databases.

Step:2.Setting up Cloud Database(AWS RDS) where the migrated database from onprem MySQL will be stored on cloud.

Step:3.Setting up AWS DMS for the migration process.

Step1:
  -> Use virtual machines like virtualbox or VMware for running the ubuntu server.
  
  ->install the MySQL server on the ubuntu server .
  
  ->After installing MySQL ,login as root user and password will be admin by default.
  
          ->Login to MySQL as root : mysql -u root -p

  ->Create a new User with remote access and give all the privileges to the user.
  
         ->Create a new user: CREATE USER 'username'@'host' IDENTIFIED BY 'password';
         
         ->Grant privileges to the user: GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' WITH GRANT OPTION;
         
         ->After granting privileges, it's important to run the FLUSH PRIVILEGES command to ensure that:FLUSH PRIVILEGES;

  ->For GUI access of  MySQL DB server use mysqlworkbench :
  
  ->import a sample database in the MySQL server so that it can be used for the migration process.

Step 2:
   ->Creating a AWS RDS DB for storing the migrated DB from the onprem MySQL server.
   
       ->login to your aws console and open the rds service.
       
       ->Click create database button.
       
   ![image](https://github.com/user-attachments/assets/de80c9e7-7eaf-4750-a197-2cec20234256)
       
       ->Choose a database creation method:  Standard create
       
       ->Engine options:  MySQL
       
  ![image](https://github.com/user-attachments/assets/cc11f2d5-2f19-42ef-af01-3470f007b1c4)
       
![image](https://github.com/user-attachments/assets/6de6fc25-9b42-4228-89d4-fbf2666aa9b4)

       ->Templates:  Free Tier
       
![image](https://github.com/user-attachments/assets/189fcbd4-5a81-4fa9-87a3-fda11f52a0af)

       -> Settings: ->DB instance identifier:DB name of ur choice.
       
                           ->Credentials Settings:
                           
                                 ->Master username:admin(ur choice)
                                 
                                 ->Credentials management:self managed
                                 
                                 -> set password
                                 
![image](https://github.com/user-attachments/assets/311e7c3c-93c8-4210-9022-0056b26fabc0)

        ->Connectivity:->public access: yes
        
![image](https://github.com/user-attachments/assets/344de446-6bf4-445d-b16a-fba865082b50)

        ->Database authentication:Password authentication
        
        ->for all other leave with default values or change according to ur requirements.
        
        ->click create database.

Step 3:

     -> Creating the replication instance,source and target endpoints, migration task using the values from step and step2 for the migrating onprem sql db to aws rds db.

     1->Creating Replication instance:
     
         ->Click Create Replication Instance.
         
![image](https://github.com/user-attachments/assets/23c8faa9-0658-4a34-a1fb-a81f6883ec2d)

         ->Settings: ->name:ur choice for naming the instance.
         
                            ->Description:optional.
                            
![image](https://github.com/user-attachments/assets/4165de60-0bf6-448e-b52b-e6c836ac8357)

        ->for Instance configuration, Storage,Connectivity and security ,Maintenance use default values or change according to ur requirements.
        
![image](https://github.com/user-attachments/assets/d31ba73f-ae7b-436f-a472-0f2e2ac22077)

![image](https://github.com/user-attachments/assets/730e9d86-95e7-4a47-b6e9-104adcca3607)

        ->click create replication instance.
        
     2->Creating Target and Source Endpoints: 

![image](https://github.com/user-attachments/assets/a9c95be6-46f0-44a6-a803-ee5cae26d5dc)

         ->TARGET END POINT:
         
             ->click create endpoint button.
             
             ->Endpoint type:->Target endpoint.
             
                                         ->check the Select RDS DB instance.
                                         
                                         ->choose the rds db instance u have created in the second step.
                                         
             ->Endpoint configuration:->Endpoint identifier:ur choice.
             
                                                       ->Target engine:mysql
                                                       
                                                       ->Access to endpoint database:Provide access information manually
                                                       
                                                       ->Server name: rds name or endpoint
                                                       
                                                       ->port:3306
                                                       
                                                      ->username and password: the one u have set while creating rds db.
![image](https://github.com/user-attachments/assets/64db271f-4ffa-4340-b829-0b2714b09e52)

![image](https://github.com/user-attachments/assets/20ade540-b99e-446a-bf3f-53ca9ee4ea09)

               ->Test endpoint connection:run test:successful
               
![image](https://github.com/user-attachments/assets/0ef59eb8-b809-408c-907c-2ce38c79e7ea)
               
               ->click create end point.
           
           ->SOURCE END POINT:
           
             ->click create endpoint button.
             
             ->Endpoint type:->source endpoint.

             ->Endpoint configuration:->Endpoint identifier:ur choice.
             
                                                       ->Target engine:mysql
                                                       
                                                       ->Access to endpoint database:Provide access information manually.
                                                       
                                                       ->Server name: public ip of vm where ur server resides.
                                                       
                                                       ->port:3306
                                                       
                                                      ->username and password: the one u have set while creating ur user in mysql

  ![image](https://github.com/user-attachments/assets/e9ba58b9-839d-49b2-8f20-e8b7ae03d256)

  ![image](https://github.com/user-attachments/assets/48491bc1-0f17-44e7-9806-ca404020ca00)


               ->Test endpoint connection:run test:successful
![image](https://github.com/user-attachments/assets/8ba197e1-1944-4527-a76b-f869fd48a6a0)

               
               ->click create end point.

        3->Creating Database migration Tasks.
        
            ->click create task.
            
            ->Task configuration:->Task identifier:ur choice
                                               ->Replication instance:choose the created crct instance.
                                               
                                               -> Source database endpoint:Choose the crct source end point(onprem db) 
                                               
                                               ->Target database endpoint:Choose the crct target end point(cloud rds db) 
                                               
                                               ->Migration type:ur choice.
                                               
![image](https://github.com/user-attachments/assets/6ccab5c9-4946-44e5-91f3-487b287a08bb)

![image](https://github.com/user-attachments/assets/240355cc-f8e8-4bde-a4ef-801548e3a0ff)


            ->Task settings:->everything default values or ur choice based on ur requirements.
            
                                       ->Maximum LOB size (KB):64kb minimum.
                                       
            ->Premigration assessment :ur wish in my case i turned off
            
            ->Migration task startup configuration:Automatically on create

  ![image](https://github.com/user-attachments/assets/af1450df-05cc-454b-a62e-c4e36ee9b9d3)

            
            -click create task.

after creating it will migrate the slected db or db's from onprem ubuntu server to aws rds if there are no errors while ur setting up the steps.

After creation of all verify the output and make some changes in the on premises db to see the on going changes in the migration task and changes reflected in cloud db on changing in on premises db

outputs or result:

-> on prem db with a example database called "sakila"

![image](https://github.com/user-attachments/assets/9ce12fc5-5152-4621-b608-e7b42dfd8e6f)

-> after the initial migration while creation there will only 16 tables in the sakila database. now create a simple table called 'sample' to see the ongoing changes

![image](https://github.com/user-attachments/assets/3cfc8cb9-f001-46d2-94db-0980d98260b4)

-> after creation of table called sample in sakila db in on premises server there will be changes in the migration task

![image](https://github.com/user-attachments/assets/04cacffc-48e8-406f-881f-946003e94e1f)

->the changes will reflected in the clouddb in rds which we connnected using sql workbench here we can see the sample table get migrated .

![image](https://github.com/user-attachments/assets/045e9ab4-1ddf-4e9d-90e9-3a9d059f9604)






                                               

