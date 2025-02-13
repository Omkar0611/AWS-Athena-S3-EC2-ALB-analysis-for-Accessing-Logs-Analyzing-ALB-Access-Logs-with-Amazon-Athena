# AWS-Athena-S3-EC2-ALB-analysis-for-Accessing-Logs-Analyzing-ALB-Access-Logs-with-Amazon-Athena
Step 1: Launch Two EC2 instance
Start by clicking Launch Instance on the EC2 Dashboard
![image](https://github.com/user-attachments/assets/918c2ca4-ce50-4696-ac7b-b2c85700abdc)
![image](https://github.com/user-attachments/assets/2e8a03b7-9535-41b5-86eb-b078f0f828c0)
Second instance
![image](https://github.com/user-attachments/assets/6d5a0645-fc5f-46ee-aefa-01a925fbe8d8)
![image](https://github.com/user-attachments/assets/cd7b774e-c44f-4aef-8e2a-35e2ed1ecc58)
This are my two instance 
![image](https://github.com/user-attachments/assets/f814b9c2-a161-4cb5-9e9c-7b2971007627)
Connect the both instance and run commands:
sudo yum update
sudo yum upgrade
sudo yum install httpd
sudo systemctl start hhtpd
sudo systemctl enable httpd
cd var/www/html
sudo vi index.html
Put simple html code inside index file
Open your browser and paste the public IPv4 address of the Server-A
![image](https://github.com/user-attachments/assets/3f3ac102-8635-45a1-84f3-e7875d89f97e)
Also verify that Server B is also functioning correctly
![image](https://github.com/user-attachments/assets/61ae71c4-c7b5-44d1-b648-b002707ffe12)
Step 2: Create Target Group
In the EC2 Dashboard locate the Load Balancing section in the left-hand navigation panel and select Target Groups.
![image](https://github.com/user-attachments/assets/13442f6c-5f98-4cf0-9654-bd476c20b246)
Click the Create target group button to start the target group creation process ensure that you have selected the instances as target type
![image](https://github.com/user-attachments/assets/3f674476-36ea-4192-9bea-1d6ec9c9323d)
• Name: Enter a descriptive name for your target group.
• Protocol: Choose the protocol (HTTP) based on your application needs.
• Port: Specify the port that your instances are listening on.
• VPC: Choose the Virtual Private Cloud (VPC) where your instances are located
![image](https://github.com/user-attachments/assets/30ff05f7-893c-44cd-96db-d977ff057ba0)
![image](https://github.com/user-attachments/assets/b037dabb-5458-4b0a-891a-06495e236749)
Register Targets
Under the Targets section, will need to register instances that should be part of this target group.
Select the instances by their instance ID or IP address and click on include as pending below
![image](https://github.com/user-attachments/assets/955d0f0a-4531-4026-87c3-718db5a9cfca)
Click on create target group
![image](https://github.com/user-attachments/assets/0c55a4a7-2d8d-4c5b-8c1e-5a280ad8b39a)
Target group is created.
![image](https://github.com/user-attachments/assets/398314c3-58c9-4706-837d-d7b451ce0730)
Step 3 : Create a Load Balancer
Click the Create Load Balancer button to start the load balancer creation process.
![image](https://github.com/user-attachments/assets/e2759b5d-25d4-4fca-a36f-8beab21600c5)
Select the type of load balancer you want to create:Choose the appropriate type for your use case and click Create
![image](https://github.com/user-attachments/assets/dd41317a-444b-4790-b030-089eb149a660)
![image](https://github.com/user-attachments/assets/67f0c47e-f532-4509-ae56-596bddb84d5a)
![image](https://github.com/user-attachments/assets/ec282beb-113e-4a95-a872-57925add985f)
Review the configuration details to ensure they are correct. Once you are satisfied click the Create button.
![image](https://github.com/user-attachments/assets/cdd50310-9dd1-4e10-a174-c62ea22d685d)
You should see a confirmation message indicating that your load balancer has been created successfully.
Your load balancer is now ready to distribute traffic across the register instances in the target group, providing high availability and scalability for your application.
Make sure to update your DNS or application configurations to point to the load balancer  DNS name or IP address.
![image](https://github.com/user-attachments/assets/6889a5bd-d5d6-4908-8659-73065deca040)
Upon refreshing the page, you will receive a response from Server A and B demonstrating that the load balancer successfully 
directs traffic to both EC2 instances ensuring redundancy and optimal load distribution.
![image](https://github.com/user-attachments/assets/b1fbeea3-ceab-414e-8bae-239aadff80cb)
![image](https://github.com/user-attachments/assets/32d2f6b4-c28f-4b26-b76a-1540dfae82e0)
Step 3 : Configure Access Logs
To enabling access logs for your Application Load Balancer ALB you will also need to create an Amazon S3 bucket and set up a policy 
to allow the ALB to write logs to the bucket.Navigate to the S3 service
Create the bucket
![image](https://github.com/user-attachments/assets/6b3a9b6b-1205-496d-a42e-e461997f0513)
![image](https://github.com/user-attachments/assets/deee5b68-e2d1-4835-903a-ac07f140c29b)
Review your bucket settings and if everything looks correct  click the Createbucket button.
![image](https://github.com/user-attachments/assets/26dba76c-ea3f-48d3-ae53-422ee35573e2)
The bucket creation process has been completed successfully. Once the bucket is created create 3 folders /prefix /Awslogs/ (youraccountid)for logs.
Step 4: Set Up a Bucket Policy After creating the S3 bucket you will need to set up a bucket policy to allow the ALB to write logs to it
In the bucket policy editor you will need to define a policy that grants the necessary permissions to the ALB.
![image](https://github.com/user-attachments/assets/0e52633f-0bd1-48c4-82ec-d8d0b92f1730)
Step 5: Enabling logs for an Application Load Balancer
Click on the name of your Application Load Balancer (ALB) for which you want to enable access logs.
Inside the ALB details page, navigate to the Attributes tab and find the Access logs section.
![image](https://github.com/user-attachments/assets/fcdf54ec-e486-4acd-bda8-21cd69af34b8)
In the Access logs section do the following Navigate to the Monitoring section by scrolling down the page Specify the S3 bucket where you want to store the access logs. 
You can choosean existing bucket or create a new one
Click the Save button to save your access log configuration.
![image](https://github.com/user-attachments/assets/f631fcad-33bb-47bb-a44f-dda1fe91804d)
From this point forward your ALB will start generating access logs and storing them in the specified S3 bucket. You can use these logs for monitoring and analysis of traffic to your load balancer.
Now check in the s3 bucket you will receive the ELBAccessLogTestFile
![image](https://github.com/user-attachments/assets/341a7825-6a36-497d-806d-b1d5883e9d60)
Now you can get the log access log in s3
![image](https://github.com/user-attachments/assets/f19fcfb9-8f44-451f-b57d-abab3daba73e)
Step 6: Querying AWS Application Load Balancer (ALB) access logs using
Amazon Athena:
1) Navigate to the Athena service by clicking on Services and then selecting Athena under the Analytics section.
2)Make sure your ALB access logs are being delivered to the S3 bucket as previously configured.
3) Create a folder in the same s3 bucket for the Athena query results.
![image](https://github.com/user-attachments/assets/00dec03c-e7f5-4b5d-8a1a-dc7ae69a536b)
In the Athena console click on Query Editor on the left-hand side Click on settings and manage Access the settings menu and then choose Manage.
From there select the S3 bucket you have designated for storing the ALB logs.click on browse select the bucket and folder and which we have created earlier steps
![image](https://github.com/user-attachments/assets/cb7d498a-f596-4e82-a0aa-5788f812c733)
Click on Choose and Save
![image](https://github.com/user-attachments/assets/f0a58e61-d9e7-49dc-89ab-72dfa0020bb9)
Step 8: Create a Table After creating the database you need to create a table that defines the structure of your ALB access logs.
You can do this with a CREATE TABLE statement. 
![image](https://github.com/user-attachments/assets/3bcbfa90-ab85-4fba-b12a-8e981160b84e)
To preview a table in Amazon Athena:
1) Click on the desired table name in the database list.
2) Open the table menu by clicking the three dots next to the name.
3) Choose Preview Table to view a sample of the tables data in a new tab or panel.
![image](https://github.com/user-attachments/assets/1b91972a-0cad-4afe-8281-dabc7151519c)
query that performs a count of HTTP GET requests grouped by the client Ip address
![image](https://github.com/user-attachments/assets/0e08d88b-9d25-4d1e-bd1b-05359157fbad)






