# **Deploy a high-availability web app using CloudFormation** `project by Patrick Coutinho`

## Udacity Cloud DevOps Engineer Nanodegree

---

## Initial steps:

- To get started set the `EnvironmentName` in the `json` files.
- Setup `BucketName` in `s3.json` and `servers.json`.

## Stack creation:

First create the networks:

```
./create.sh IaCPatrickUdacityProjectNet network.yml network.json
```

Then create the bucket:

```
./create.sh IaCPatrickUdacityProjectS3 s3.yml s3.json
```

Copy the index.html into the bucket::

```
aws s3 cp index.html s3://iacpatrickudacityproject-umteste2/index.html
```
Create the servers:
```
./create.sh IaCPatrickUdacityProjectServers servers.yml servers.json
```

Copy the DNS address of the load balancer with:
```
./describe.sh IaCPatrickUdacityProjectServers
```

---

### Server specs

1. You'll need to create a **Launch Configuration** for your application servers in order to deploy four servers, two located in each of your private subnets. The launch configuration will be used by an auto-scaling group.

2. You'll need two vCPUs and at least 4GB of RAM. The Operating System to be us]ed is Ubuntu 18. So, choose an Instance size and Machine Image (AMI) that best fits this spec.

3. Be sure to allocate at least 10GB of disk space so that you don't run into issues.

### Security Groups and Roles

1. Since you will be downloading the application archive from an S3 Bucket, you'll need to create an IAM Role that allows your instances to use the S3 Service.

2. Udagram communicates on the default `HTTP Port: 80`, so your servers will need this inbound port open since you will use it with the Load Balancer and the Load Balancer Health Check. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.

3. The load balancer should allow all public traffic `(0.0.0.0/0)` on port `80` inbound, which is the default `HTTP port`. Outbound, it will only be using `port 80` to reach the internal servers.

4. The application needs to be deployed into private subnets with a **Load Balancer** located in a public subnet.

5. One of the output exports of the **CloudFormation** script should be the public URL of the **LoadBalancer**. **Bonus points** if you add `http://` in front of the load balancer **DNS Name** in the output, for convenience.