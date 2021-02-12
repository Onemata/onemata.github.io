## Configure AWS IAM Role
`ONE-SALES-PR-21002` Updated: 2021-Feb-10

Many clients will access Onemata provided data using AWS services such as:

- Redshift
- Glue/Athena
- EC2 (Elastic Compute Cloud)

To best protect your sensitive data and safeguard your AWS access credentials, we recommend creating an IAM role and attaching it to your service runtimes, instances and clusters. 

### Configuring AWS IAM Role's for Redshift
For any operation that accesses data on another AWS resource, your cluster needs permission to access the resource and the data on the resource on your behalf. An example is using a COPY command to load data from Amazon S3. You provide those permissions by using AWS Identity and Access Management (IAM). You do so either through an IAM role that is attached to your cluster or by providing the AWS access key for an IAM user that has the necessary permissions. For more information about credentials and access permissions, see [Credentials and access permissions](https://docs.aws.amazon.com/redshift/latest/dg/loading-data-access-permissions.html).

To best protect your sensitive data and safeguard your AWS access credentials, we recommend creating an IAM role and attaching it to your cluster. For more information about providing access permissions, see [Permissions to access other AWS resources](https://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-access-permissions.html).

In this step, you create a new IAM role that enables Amazon Redshift to load data from Amazon S3 buckets. In the next step, you attach the role to your cluster.

#### To create an IAM role for Amazon Redshift
1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

2. In the navigation pane, choose **Roles**.

3. Choose **Create role**.

4. In the AWS Service group, choose **Redshift**.

5. Under **Select your use case**, choose **Redshift - Customizable** then choose **Next: Permissions**.

6. On the **Attach permissions policies** page, choose **AmazonS3ReadOnlyAccess**. You can leave the default setting for **Set permissions boundary**. Then, choose **Next: Tags**.

7. The **Add tags** page appears. You can optionally add tags. Choose **Next: Review**.

8. For **Role name**, enter a name for your role. For this tutorial, enter `onemata-redshift`.

9. Review the information, and then choose **Create Role**.

10. Choose the role name of the role you just created.

11. Copy the **Role ARN** to your clipboard—this value is the Amazon Resource Name (ARN) for the role that you just created. You use that value when you use the COPY command to load data into Redshift. You'll need to provide this to Onemata as well. 

12. Now that you have created the new role, your next step is to attach it to your cluster. You can attach the role when you launch a new cluster or you can attach it to an [existing cluster](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum-add-role.html).


### Configuring AWS IAM Role's for AWS Glue/Athena

You need to grant your IAM role permissions that AWS Glue can assume when calling other services on your behalf. This includes access to Amazon S3 for any sources, targets, scripts, and temporary directories that you use with AWS Glue. Permission is needed by crawlers, jobs, and development endpoints.

You provide those permissions by using AWS Identity and Access Management (IAM). Add a policy to the IAM role that you pass to AWS Glue.

#### To create an IAM role for AWS Glue

1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

2. In the left navigation pane, choose **Roles**.

3. Choose **Create role**.

4. For role type, choose **AWS Service**, find and choose **Glue**, and choose **Next: Permissions**.

5. On the **Attach permissions policy** page, choose the policies that contain the required permissions; for example, the AWS managed policy **AWSGlueServiceRole** for general AWS Glue permissions and the AWS managed policy **AmazonS3FullAccess** for access to Amazon S3 resources. Then choose **Next: Review**.

6. For **Role name**, enter a name for your role; for example, **AWSGlueServiceRoleDefault**. Create the role with the name prefixed with the string **AWSGlueServiceRole** to allow the role to be passed from console users to the service. AWS Glue provided policies expect IAM service roles to begin with **AWSGlueServiceRole**. Otherwise, you must add a policy to allow your users the **iam:PassRole** permission for IAM roles to match your naming convention. Choose **Create Role**.

### Configuring AWS IAM Instance Profile's for EC2

> **Please provide your AWS Account Number to Onemata before configuring Instance Profiles. Onemata will then provide you with required information for the configuration such as the ARN to assume, and the newly accessible S3 Bucket.**

1.    Sign in to the AWS Management Console.

2.    Open the IAM console. https://console.aws.amazon.com/iam/

3.    From the navigation pane, choose **Roles**.

4.    Choose **Create role**.

5.    For Select type of trusted entity, choose **AWS service**.

6.    For **Choose the service that will use this role**, choose **EC2**.

7.    Choose **Next: Permissions**.

8.    Choose **Next: Tags**.

9.    You can add optional tags to the role. Or, you can leave the fields blank, and then choose **Next: Review**.

10.    For **Role name**, enter a name for the role.

11.    Choose **Create role**.

12.    From the list of roles, choose the role that you just created.

13.    Choose **Add inline policy**, and then choose the **JSON** view.

14.    Enter the following policy. Replace `arn:aws:iam::111111111111:role/ROLENAME` with the Amazon Resource Name (ARN) of the IAM role that Onemata provides.
```
{ 
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::111111111111:role/ROLENAME"
        }
    ]
}
```
15.    Choose **Review policy**.

16.    For **Name**, enter a name for the policy.

17.    Choose **Create policy**.

18.    [Attach the IAM role](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) (instance profile) to the Amazon EC2 instance that you use to access the Amazon S3 bucket.

#### From the Amazon EC2 instance, configure the role with your credentials

1.    Connect to the Amazon EC2 instance. For more information, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) or [Connecting to your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html).

2.    After you connect to the instance, verify if the directory already has a folder named **~/.aws**. To find this folder, you can run a command that lists the directory, similar to the following:
```
ls -l ~/.aws
```
3.    If you find the **~/.aws** folder, proceed to the next step. If the directory doesn't have an **~/.aws** folder yet, create the folder by running a command similar to the following:
```
mkdir ~/.aws/
```
4.    Within the **~/.aws** folder, use a text editor to create a file. Name the file **config**.

5.    In the file, enter the following text. Replace **profilename** with the name of the role that you attached to the instance. Then, replace **arn:aws:iam::111111111111:role/ROLENAME** with the ARN provided by Onemata.

```
[profile profilename]
role_arn = arn:aws:iam::111111111111:role/ROLENAME

credential_source = Ec2InstanceMetadata
```

6.    Save the file.

#### Verify the instance profile

To verify that your instance's role (instance profile) can assume the role Onemata provides, run the following command while connected to the instance. Replace **profilename** with the name of the role that you attached to the instance.
```
$ aws sts get-caller-identity --profile profilename
```
The command returns a response similar to the following:
```
"Account": "11111111111",

 "UserId": "AROAEXAMPLEID:sessionName",

 "Arn": "arn:aws:sts::111111111111:assumed-role/ROLENAME/sessionName"
 ```
Confirm that the value for **"Arn"** matches the ARN of the role that Onemata provides.

#### Verify access to the Amazon S3 bucket
To verify that your instance can access the Amazon S3 bucket, run this list command while connected to the instance. Replace **profilename** with the name of the role that you attached to the instance.
```
aws s3 ls s3://DOC-EXAMPLE-BUCKET --profile profilename
```
If your instance can access the bucket successfully, you receive a response that lists the contents of the bucket, similar to the following:
```
PRE Hello/
2018-08-15 16:16:51 89 index.html
```

### AWS Documentation Reference: 
- [Creating an IAM Role for Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-create-an-iam-role.html)
- [Creating an IAM Role for AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html)
- [How can I grant my Amazon EC2 instance access to an Amazon S3 bucket in another AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/s3-instance-access-bucket/)

![](https://www.onemata.com/hs-fs/hubfs/Logos/Onemata%20Logo%20-%20wide.png)


### Support or Contact

[Contact support](https://www.onemata.com/contact)
