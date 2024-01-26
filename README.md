
# Static Website Hosting on AWS S3 Cloud Front 

In this project, I have deployed a simple website on Amazon Web Services (AWS) S3 (Simple Storage Service) and Clod Front.

I have also utilized Route 53 to register my own domain name for accessing the website. Additionally, I employed CloudFront and TLS to secure and distribute the content globally.

The IAM Management Console was used for project execution.

# IAM policy attached to the IAM User

AmazonRoute53FullAccess

AmazonS3FullAccess	

AWSCertificateManagerFullAccess	

CloudFrontFullAccess	

IAMUserChangePassword

## Bucket policy

```bash
  {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"cloudfront:CreateDistribution",
				"cloudfront:GetDistribution",
				"cloudfront:ListDistributions",
				"cloudfront:TagResource",
				"cloudfront:UpdateDistribution"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": "wafv2:CreateWebACL",
			"Resource": "arn:aws:wafv2:us-east-1:037230611102:global/managedruleset/*/*"
		}
	]
}

```
## Tech Stack

**AWS** : IAM , S3 , CloudFront , IAM , Route 53

**HTML** 

**CSS** 


## Step : 1 Configure AWS S3 Bucket

I will select US EAST (N. Virginia) as my region, as it is near my location. Although S3 is not region-specific, selecting a nearby region will help with low latency.

Public Access: Block Public Access settings are disabled for internet access, although it is not a recommended practice for S3.

Bucket Versioning: Enabled for file backup in case of accidental deletion.

Encryption: AWS enables server-side encryption by default.

Static Website Hosting: Enabled to host the website on AWS S3. Necessary files related to the website, including the index.html file, are uploaded during Static Website Hosting configuration.



![Screenshot (523)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/73a8342d-a429-4c95-bbe4-0afdc324e9cc)


![Screenshot (513)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/1d61cff9-9f00-4192-b867-baeb75fe48fb)
![Screenshot (514)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/3da8f33a-bc18-4598-b8a3-21cf5dbb2c4a)
![Screenshot (515)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/68e54176-be29-432f-83a5-acf027455de0)
![Screenshot (516)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/7c81b0a9-3dd9-4c32-87fe-0a8830cf993b)

Bucket policy : To access the website from internet we have to add the Bucket policy accordingly.

## Bucket policy

```bash
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::harshilrpatel.com/*"
        }
    ]
}
```

The above bucket policy allows access to the website for everyone. The website is hosted on AWS S3 and accessible through the Bucket website endpoint.


## Step : 2 Register a Domain Name

To access the website through a domain name, I purchased a domain name using AWS Domain Registry service. The domain name costed USD 13.00 for a year.

Domain Name : harshilrpatel.com

![Screenshot (517)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/31c85baf-e082-4dfa-9f32-b6aac8f73531)
## Step : 3 Add CloudFront distribution with SSL Certificate

Although the website is accessible from my domain name, it shows as "NOT SECURE" on the browser due to the lack of an SSL certificate. 

![Screenshot (494)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/d5ed0c78-d932-4e1e-a55b-9a718a7ab678)

I obtained an SSL certificate and configured the CloudFront distribution.

![Screenshot (521)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/9f3b6dd4-af06-483a-a84c-b10a32b223e5)
![Screenshot (522)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/92ea0861-d55a-40bf-a9d6-fd409bb670c0)
![Screenshot (523)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/ab01dffd-5193-4b8f-8cae-828070be73ef)

Clou Front Distribution 

Origin Domain: Amazon S3 Bucket (Where Website is Hosted)

Viewer Policy Control: Redirect HTTP to HTTPS

Settings: Utilized all edge locations, Alternate domain name (CNAME): harshilrpatel.com, Custom SSL certificate added.

![Screenshot (519)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/0e441953-fe84-4514-8775-0bb56f90d074)

![Screenshot (520)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/8dd98766-c6d2-41bf-8d27-95a4e17e5388)

## Step : 4 configure the Hosted Zone Records

I created a record to ensure the CloudFront distribution is connected to the Route 53 Domain Name.

Record Type: A - Route traffic to an IPv4 address and some AWS resources.

Value/Route traffic to: Alias to CloudFront Distribution, AWS region added.

![Screenshot (518)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/133529ca-456d-46ed-8e11-efa94c4845a3)


After creating the record, the website is accessible with the domain name harshilrpatel.com.

![Screenshot (509)](https://github.com/harshilp156/Static-Website-Hosting-on-S3-CloudFront/assets/67538347/0d2253c4-f97a-432f-987b-72fa3362caca)
