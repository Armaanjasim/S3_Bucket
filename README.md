# Amazon Simple Storage Service (S3)
![Disaster_Recovery](DR.PNG)

# Table of Contents
- [Amazon Simple Storage Service (S3)](#amazon-simple-storage-service-s3)
- [Table of Contents](#table-of-contents)
  - [What is Amazon S3](#what-is-amazon-s3)
  - [Why use Amazon S3](#why-use-amazon-s3)
  - [Accessing Amazon S3](#accessing-amazon-s3)
  - [Buckets on Amazon S3](#buckets-on-amazon-s3)
  - [Using boto3 For Amazon S3](#using-boto3-for-amazon-s3)

## What is Amazon S3
Amazon S3 is a storage service offered by amazon which allows storage of any format. The service offers:
- Scalability
- Data availability
- Security  

[Back](#table-of-contents)

## Why use Amazon S3
Amazon S3 can be used:
- To access databases on AWS
- To allow data to be globally available
- To store any format of data
- To backup data on more than one server
 
[Back](#table-of-contents) 

Amazon S3 can also be used as a disaster recovery plan.

## Accessing Amazon S3
- Enter the EC2 Instance **LINK**
- Amazon S3 uses python3 however ubuntu by default uses 2.7 so first we must enter these commands to get python 3 working in our instance so we must enter these commands:
  - `sudo apt install python3-pip`
  - `sudo apt install python3.7-minimal`
  - `alias python=python3.7`
  - `sudo pip3 install awscli`
  - `aws configure`
    - access key: **KEY**
    - secret access key: **KEY**
    - region name: `eu-west-1`
    - output format: `json`
  - `aws s3 ls` it should show you the files you have on the service if everything is done correctly  

[Back](#table-of-contents)


## Buckets on Amazon S3
- Making a bucket
  - `aws s3 mb s3://eng103a-armaan-devops`
    - "eng103a-armaan-devops" is the name of the folder I want to create.
    - S3 does not allow "_" or capital letters
- Removing a bucket 
  - `aws s3 rb s3://eng103a-armaan-devops`
    - there must be nothing in the bucket in order to delete the bucket
- Copying data from ec2 to a bucket:
  - `aws s3 cp test.txt s3://eng103a-armaan-devops`
- Copying data from a bucket to ec2:
  - `aws s3 cp s3://eng103a-armaan-devops/test.txt ~`

[Back](#table-of-contents)

## Using boto3 For Amazon S3
- First boto3 would have to be install for this pip and python must also be installed:
  - `sudo apt install python3`
  - `sudo apt install pip3`
  - check the version by `pip --version`
  - `pip3 install boto3`
- once this is done you can make a file called s3.py and in this file you must import boto3 in order to :
```
#!/usr/bin/env python
import boto3
bucket_name = "eng103a-armaan-devops"
location = {'LocationConstraint': "eu-west-1"}
s3 = boto3.resource('s3')
bucket_name = input("S3 Bucket Name\n")

while True:
    action = input("Type:[mb] Make a bucket, [rb] Remove a bucket, [d] Delete a file, [u] Upload a file, [dl] Download a file, [e] Exit Script\n")

    if action == "rb":
        s3.Bucket(bucket_name).delete()
    elif action == "mb":
        s3.create_bucket(Bucket=bucket_name, CreateBucketConfiguration=location)
    elif action == "d":
        filename = input("what is the filename?\n")
        s3.Object(bucket_name, filename).delete()
    elif action == "u":
        filename = input("what is the name of the file you're uploading?\n")
        targetfilename = input("what is the destination file name?\n")
        s3.Bucket(bucket_name).upload_file(filename, targetfilename)
    elif action == "dl":
        filename = input("what is the name of the file you're downloading?\n")
        targetfilename = input("what is the destination file name?\n")
        s3.Bucket(bucket_name).download_file(filename, targetfilename)
    elif action == "e":
        break
    else:
        print(\n"Please enter from the following optioins please")

Inspired by Zilamo
```
- You can then run this script by typing `python3 s3.py` in the ec2 instance and make a bucket or remove etc.

[Back](#table-of-contents)
