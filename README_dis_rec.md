EC2 / S3 / DR (Disaster Recovery plan)
-

**`DR`**

It is a set of procedures and policies put in 
place by an organization to ensure that it can recover 
and continue mission-critical functions in the event of a 
disaster or other disruptive event.

Use cases include: 
- natural disasters, 
- cyber attacks, 
- power outages, 
- hardware failures

Benefits of `DR` include: 
- minimizing downtime and data loss, 
- maintaining business continuity, 
- reducing financial losses,
- ensuring regulatory compliance

Organizations use DR to protect their data, applications, and infrastructure from unexpected disruptions

**`S3` stands for Simple Storage Service.**

It is a cloud-based object storage service offered 
by Amazon Web Services (AWS).

`S3` provides a highly scalable, secure, 
and durable storage infrastructure for businesses 
and individuals to store and retrieve any amount of 
data, at any time and from anywhere on the web.

`S3` is designed to be highly available and fault-tolerant, 
with automatic data replication across multiple geographically 
distributed data centers to ensure high availability and durability of data.


`S3` is widely used for: 
- backup and recovery, 
- archiving, 
- big data analytics, 
- content distribution.

**`AWSCLI` stands for Amazon Web Services Command Line Interface.**

It is an open-source tool that enables users to interact with AWS 
services using a command-line interface.
`AWSCLI` provides users with a way to automate tasks, manage resources, and configure services on AWS.

**`SDK` stands for Software Development Kit.**
AWS offers SDKs for a variety of programming languages, including:
- Java, 
- Python, 
- Ruby.

`SDKs` provide developers with a 
way to interact with AWS services in their 
code, and they include libraries, code samples, 
and documentation to help developers get started quickly.

Setting up S3:
-

![diaa.png](files%2Fdiaa.png)


1. Create a new EC2 instance, using `Ubuntu 18.04`, 
we used the naming convention `mateusz-s3-tech221`, 
and for security groups add an SSH rule `port 22` and `my-ip` to allow you to SSH in.

2. Now make sure to follow commands:

2.1 - `sudo apt update -y` - updates package list.

2.2 - `sudo apt upgrade -y` - upgrades installed packages to latest version.

2.3 - `python --version` - python version currently installed.

2.4 - `python3 --version` - version of python3 ins tall.

2.5 - `sudo apt install python` - install python.

2.6 - `sudo apt install python3-pip` - installs pip, package installer for python3.

2.7 - `alias python=python3` - creates alias so python3 is used instead of python.

2.8 - `python --version` - should now show the python 3 version, after alias.

2.9 - `sudo apt-get install python3-pip` - installs pip.

2.10 - `sudo python3 -m pip install awscli`- installs aws command line interface using pip.

2.11 - `aws configure` - configures aws cli, and when prompted enter AWS ACCESS KEY, SECRET ACCESS KEY, REGION AND OUTPUT FORMAT (secret key and access key should be found in your .ssh folder upon receiving).

2.12 - We enter our `access` and `secret keys`, and for region we enter `eu-west-1` and output format as `json`.

2.13 - Once you enter, run `aws configure` to check if changes have been saved.

2.14 - `aws s3 ls` - when running this it should show all objects in the S3 bucket, which should confirm `AWS CLI` has been configured correctly.

2.15 - We then use `aws s3 mb s3://nameofbucket` in the aws cli, name we used for name of bucket is mateusz-tech221 use this naming convention.

2.16 - Bucket should now be made and be present on the `S3 interface` on AWS -> navigating to buckets on the interface.

2.17 - We make a `test file` using `sudo nano test.txt` and enter some information in it using `#`, `save` and `exit`.

2.18 - We then use `aws s3 cp test.txt s3://mateusz-tech221`, to upload the file to our bucket.

2.19 - Navigating through the S3 interface -> buckets -> , it should show the file available within the bucket as shown below.

![CLI_1.png](files%2FCLI_1.png)

2.20 - If the file on local host is deleted (via `sudo rm test.txt`), we can use `aws s3 cp s3://bucket-name/filename filename`.

2.21 - In our case it was aws `s3 cp s3://mateusz-tech221/test.txt test.txt`, once running this we can see the file was downloaded from the bucket again and after running `cat` we can see the contents are the same as before.

2.22 - `aws s3 sync s3://bucket-name/ test.txt` - can also be used if there are a lot of files and you want to sync them all.

2.23 - `aws s3 rm s3://bucket-name/test.txt` - deletes object in the bucket, so the bucket is now empty, but the bucket itself is there.

2.24 - `aws s3 rb s3://bucket-name` - removes the bucket itself if empty, if not empty it would not allow you to delete.

S3 Scripting (boto3):
-

1. We need to make sure that our instance has all the dependencies for boto3:

- `sudo apt install python-pip`

- `pip3 install boto3`

2. Create a test file we make one with random text called `test_script.txt`.
3. We then create 5 python scripts - `create_bucket.py`, `upload_file.py`, `download_file.py`, `delete_bucket_objects.py` and `delete_bucket.py`.
4. For `create_bucket` the code is:

```commandline
import boto3

 

s3 = boto3.resource('s3')
s3.create_bucket(Bucket='mateusz-tech221-boto3', CreateBucketConfiguration= {'LocationConstraint': 'eu-west-1'})
```

5. Upload_file:

```commandline
import boto3

 

# Create an S3 client
s3_client = boto3.client('s3')

 

# Upload the file to S3
s3_client.upload_file("test_script.txt", "mateusz-tech221-boto3", "test_script.txt")
```

6. Download_file:

```commandline
import boto3

 

# Create an S3 client
s3_client = boto3.client('s3')

 

# Download the file from S3
s3_client.download_file("mateusz-tech221-boto3", "test_script.txt", "test_script.txt")
```
7. Delete_bucket_objects:

```commandline
import boto3

 

# Create an S3 resource
s3 = boto3.resource('s3')

 

# Delete all objects in the bucket
bucket = s3.Bucket('mateusz-tech221-boto3')
bucket.objects.all().delete()
```
8. Delete_bucket:

```commandline
import boto3

 

# Create an S3 resource
s3 = boto3.resource('s3')

 

# Delete all objects in the bucket
bucket = s3.Bucket('mateusz-tech221-boto3')
bucket.objects.all().delete()
```
9. **To run the scripts, we use `python <py_script_name>.py`, and it should run the script needed, and should be reflected on the S3 interface.**











