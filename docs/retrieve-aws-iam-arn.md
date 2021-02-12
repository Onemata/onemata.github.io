## Retrieve an AWS IAM User or Role ARN
`ONE-SALES-PR-21003` Updated: 2021-Feb-10

Amazon Resource Names (ARNs) uniquely identify AWS resources in the AWS environment. They're especially used in IAM and S3 Bucket policies.

> **Note:** Onemata cannot accept or share AWS Access Keys and clients can provide ARN's for secure access to AWS environments.

### Retrieve role or user's ARN via AWS Console

1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

2. In the left navigation pane, choose **Users** or **Roles**.

3. Select the user or role name to view a page that displays its **Arn**

4. Find the User ARN or Role ARN and copy it to the clipboard or note it down.

### Retrieve role or user's ARN via AWS Command Line Interface (CLI)

1.    Connect to the Amazon EC2 instance. For more information, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) or [Connecting to your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html).

2.    After connecting to the instance, in a terminal or command prompt run `aws sts get-caller-identity` and note the **"Arn"** in the response similar to below:

```
{
    "UserId": "AIDASAMPLEUSERID",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/DevAdmin"
}
```

Additionally, to find another user's Arn run a command similar to `aws iam get-user --user-name Paulo` replacing Paulo with a known username.


![](https://www.onemata.com/hs-fs/hubfs/Logos/Onemata%20Logo%20-%20wide.png)


### Support or Contact

[Contact support](https://www.onemata.com/contact)
