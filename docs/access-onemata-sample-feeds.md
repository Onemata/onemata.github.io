## Access Onemata Sample Feeds
`ONE-SALES-PR-21004` Updated: 2021-Feb-10

Onemata provides access to samples of data feeds available. Accessing Sample Feeds is similar to accessing Standard Production Onemata feeds. The main difference is the endpoint and quantity of files available. 

A brief outline on how to access Sample Data Feeds :

1. Setup AWS IAM Users & Roles

2. Provide AWS Account #, IAM User & Role ARN's to Onemata team

3. Onemata provisions access to S3 Buckets and provides S3 URI and Paths and provides to client

4. Client accesses Onemata provided buckets and paths using given credentials

> **Please note: Onemata provides S3 Bucket's with **Requester Pays** enabled. The buckets require an additional parameter similar to `--request-payer requester` to view and download data. For more information please visit AWS's page about [Requester Pays S3 Bucket Configuration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RequesterPaysBuckets.html)

### Step 1: Setup AWS IAM Users & Roles

Please find our additional documentation about creating User's and Roles in AWS.

- [Configure AWS IAM User](https://onemata.github.io/configure-aws-iam-user.html)

- [Configure AWS IAM Role](https://onemata.github.io/configure-aws-iam-role.html)

### Step 2: Provide AWS Account #, IAM User & Role ARN's

Please send all AWS ARN's to Onemata to provision access. Feel free to reference our documentation about [finding your ARN's](https://onemata.github.io/retrieve-aws-iam-arn.html). 

### Step 3: Receive S3 Bucket URI and Paths

Onemata will provision access to AWS S3 Buckets and provide the S3 URI and path to requested data.

Generally Onemata uses AWS region `us-west-2` (Oregon) and names S3 Buckets using the following format:

```
s3://**feed-id**.sample.parquet.usw2.onemata.com
```

or

```
s3://**client-id**.custom.csv.usw2.onemata.com
```

We provide standard sample feeds in Snappy Parquet and Tab delimited CSV files with gzip compression. They have paths similar to: `location_country=**ISO 2 letter country code**/output_year=**year**/output_month=**month**/output_day=**day**/Onemata_**Feed_Type**_**yearmonthday**_**ISO 2 letter country code**_**batch_numbers_underscored**.gz`

**Example: /location_country=AE/output_year=2021/output_month=02/output_day=01/Onemata_Mobile_Location_Data_20210201_AE__B0000__1_000.gz**

Custom feed's paths and formats are provided matching the client's specifications.

### Step 4: Access Data Onemata Provides

Guidance differs depending on the service being used to access the data. If trying to view using the command line interface on linux ensure that you or the service being used is authenicating correctly by running a command similar to `aws sts get-caller-identity`. The output of the command includes an **ARN**, and ensure to provide to Onemata to allow access.

1. On a device authenticating with an approved ARN run `aws s3 ls s3://**feed-id**.sample.usw2.onemata.com/ --request-payer requester`. The output will show an authentication error or a list of S3 paths.

2. Run `aws s3 ls s3://**feed-id**.sample.usw2.onemata.com/location_country=**ISO 2 letter country code**/output_year=**year**/output_month=**month**/output_day=**day**/ --request-payer requester` to see a list of the given days files for the requested country. 

> **Please note:** Additional AWS IAM Access Policies may be required to access the bucket and paths given by Onemata. Please consult your AWS Administrator to update policies as needed. 

#### Additional examples

Download a given days files for a country: 
```
aws s3 cp s3://**feed-id**.sample.usw2.onemata.com/location_country=**ISO 2 letter country code**/output_year=**year**/output_month=**month**/output_day=**day**/ . --request-payer requester
```

Access Policy Example:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::AccountABucketName/*"

        }
    ]
}
```
Replace **AccountABucketName** with the bucket provided by Onemata. 


![](https://www.onemata.com/hs-fs/hubfs/Logos/Onemata%20Logo%20-%20wide.png)


### Support or Contact

[Contact support](https://www.onemata.com/contact)
