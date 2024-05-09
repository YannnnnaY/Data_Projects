# Apache Airflow

<img src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/7891216d-492a-4cf1-8df1-4a8dd989dc82" width="600" >


<img src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/7b9f5419-e180-4397-80a2-194e73504ae2" width="500" >

#### AWS Setting

EC2 

**Create Instance**

AMI: Ubuntu -> Create new key pair 

-> SELECT "Allow HTTPS traffic from the internet" and "Allow HTTP traffic from the internet"

-> Launch Instance

**Connect to Instance**

Go to the instance -> click "connect" -> "SSH client" -> copy the command and run in terminal
* make sure I'm the only user has access to the key file. Everyone else should have no access. “chmod 600 xxxxx_key.pem"

**Run the ubuntuz_commands.sh**

After get the login user name and password, go to AWS console -> your instance, copy the Public IPv4 DNS -> go to a browser and connect to the address with :8080 

* If not able to connect to the DNS address in the brower, go to the EC2 instance -> Security -> Security groups -> edit inbound rules -> add a rule 
<img width="1000" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/5e9b1fe2-3e31-4d7a-8447-3ad2147cdb19">

* AWS Trouble Shooting Doc: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html


**Deploying a DAG (Directed Acyclic Graph) script to Apache Airflow - from localhost**

1. DAG Folder Location: ls ~/airflow
2. View the Configuration to find the dags_folder: airflow config get-value core dags_folder
3. Change the dags_folder to the acutal folder name 
4. Copy dag related scripts to the assigned dags_folder
   if deploy airflow from local machine: <img width="400" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/c2b4e7bb-4b07-4082-84c8-6217cbd1bea8">
6. kick off the dag on airflow console and check the results
   <img width="1000" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/9a4e9dc0-5970-4f2a-90ae-55360c3a0cee">



# Google Cloud Platform

<img src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/398934b2-60b6-4881-a20e-d5624564194c" width="800" >


#### Compute Engine
1. Create a VM instance
2. connect to instance through SSH
3. Update the VM: 'sudo apt-get update'
4. check if python is installed


#### Cloud shell
* how to check the cloud buckets 
<img src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/48abd086-1a54-4573-adce-b46d19c7f436" width="600" >

* if error says no authorization/access to buckets, run:
  - gsutil config
  - gcloud auth login


### How to schedule a cron using py script by Cloud Functions & Pub/Sub
1. Create a bucket for the project
   * If a file couldn't be uploaded to a bucket successfully in gcp local testing env, try run this in cloud shell:
      - gcloud auth login
      - gcloud auth application-default login
   * If a file couldn't be uploaded to a bucket successfully in cloud function env, see explanation below:
      - When you run a Google Cloud Function, it runs with an identity that you get to specify.
      See here: https://cloud.google.com/functions/docs/securing/function-identity
      When you then use Google Cloud Client libraries, they implicitly know how to interact with the environment and when you make calls to them (eg. Google Cloud Storage read requests), they are implicitly authenticated as that identity.  The identity is a Service Account.  You should then grant that service account permissions applicable for what your function is doing.  Using this "Application Default Credentials" technique means that YOU don't have to mess with credentials and authentication within the logic of your own code.  When you deploy your function ... that is where you get to map your desired service account to the identity that the function will run with.

2. Create a Pub/Sub
   <img width="1000" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/2b65b03a-b906-413f-8c42-fa44612c2fd3">

3. Create a cron using Job Scheduler 
   * connect to the pub/sub created in step 2
   <img width="1000" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/42f25aed-0a5f-490f-a721-7c738a08e8b0">

4. Cloud functions:
   * Trigger by the Pub/Sub created in step 2
   * Save the scripts either in a zip folder in bucket or using the internal editor
     <img width="1000" alt="image" src="https://github.com/YannnnnaY/Data-Engineering-Projects/assets/120424783/34d035a6-7e6d-49c2-a32e-c9e8e3dfe9bf">
