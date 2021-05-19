## Mobile Location Data Feed
`ONE-SALES-JA-21001` Updated: 2021-May-17

### Overview

The Onemata Mobile Location Feed is scalable, high-fidelity device location data. With mobile location data you can gain a competitive advantage by understanding robust real world movement data that resolves devices to audiences. Device identifier and type is combined at least hourly with latitude, longitude and timestamp to provide real world human movement. Depending on the methodology various levels of accuracy are reported. Additional data that is included when available is: Device OS, User Agent, IP Address (v4 and v6), direction, and speed. 

### Column Descriptions

| Column Name          | dtype | Description                                                                                                                                                                                              |
| -------------------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mobile\_ad\_id       | str | Mobile Advertising ID                                                                                                                                                                                    |
| mobile\_ad\_id\_type | str | Android platform identifies as AAID and Apple platform as IDFA                                                                                                                                           |
| utc\_timestamp       | datetime | This field includes the UTC timestamp reported when the device location was observed.                                                                                                                    |
| country              | str | This field shows the two character country in which the event occurred.                                                                                                                                  |
| latitude             | float | When reported with the observation the latitude in decimal degrees (DD) geographic coordinates. Positive latitudes are north of the equator, negative latitudes are south of the equator.                |
| longitude            | float | When reported with the observation the longitude in decimal degrees (DD) geographic coordinates. Positive longitudes are east of the Prime Meridian; negative longitudes are west of the Prime Meridian. |
| horizontal\_accuracy | float | This field shows the horizontal accuracy associated with the GPS location of the mobile device in meters.                                                                                                |
| heading              | float | This field shows the decimal degrees from north, clockwise 0 to 360 degrees.                                                                                                                             |
| speed                | float | This field shows the speed of movement in meters/second based on GPS signal detected during the event.                                                                                                   |
| altitude             | float | Altitude associated with the GPS location of the mobile device in meters                                                                                                                                 |
| vertical\_accuracy   | float | Vertical accuracy associated with the GPS location of the mobile device in meters.                                                                                                                       |
| device\_os           | str | Represents the device operating system and version when available.                                                                                                                                       |
| user\_agent          | str | This field includes a description of the web user agent details.                                                                                                                                         |
| ipv\_4               | str | This field shows the IPv4 Address detected during the event.                                                                                                                                             |
| ipv\_6               | str | This field shows the IPv6 Address detected during the event.                                                                                                                                             |
| publisher            | str | An ID representing the publisher / app / app bundle                          |
| consent\_string      | str | IAB CCPA Framework (Framework) / (USP String) or the IAB’s GDPR solution (the Transparency and Consent Framework (TCF) |

### Example 1

| Column Name          | Example #1                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| mobile\_ad\_id       | 188CF95F-6AD6-4394-AB09-68B7B61B1829                                                                                                 |
| mobile\_ad\_id\_type | AAID                                                                                                                                 |
| utc\_timestamp       | 2021-02-15 13:11:00                                                                                                                           |
| country              | IT                                                                                                                                   |
| latitude             | 41.902782                                                                                                                            |
| longitude            | 12.496366                                                                                                                            |
| horizontal\_accuracy | 13.6                                                                                                                                 |
| heading              | 0                                                                                                                                    |
| speed                | 0                                                                                                                                    |
| altitude             | 66.2999954                                                                                                                           |
| vertical\_accuracy   | 1.7                                                                                                                                  |
| device\_os           | Android 4                                                                                                                            |
| user\_agent          | Mozilla/5.0 (Linux; U; Android 4.1.2; it-it; GT-N5100 Build/JZO54K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Safari/534.30 |
| ipv\_4               | 79.143.114.6                                                                                                                         |
| ipv\_6               | FE80::5E3C:27FF:FE3C:52BF                                                                                                            |
| publisher            | 151e0498a48d6160ad9c5d6e6bafd490                                                                                                     |
| consent\_string      | 11111110111110010100110011011111001010110                                                                                            |

### Example 2

| Column Name          | Example #2                                |
| -------------------- | ----------------------------------------- |
| mobile\_ad\_id       | 4D42B86E-44BA-4DE6-B9EC-FE18774A2DFA      |
| mobile\_ad\_id\_type | IDFA                                      |
| utc\_timestamp       | 2021-02-10 01:13:00                       |
| country              | GB                                        |
| latitude             | 53.483959                                 |
| longitude            | \-2.244644                                |
| horizontal\_accuracy |                                           |
| heading              | 331.5234375                               |
| speed                | 32.38                                     |
| altitude             |                                           |
| vertical\_accuracy   |                                           |
| device\_os           |                                           |
| user\_agent          | 13.3.1                                    |
| ipv\_4               | 77.103.41.101                             |
| ipv\_6               |                                           |
| publisher            | f7cceb4540d611ea9d900e6b4470659a          |
| consent\_string      | 11111110111110010100110011011111001011111 |

### Format & Delivery 

| Country | Minimum DAU Order | Max File Size | Parquet | CSV            |
| ------- | ----------------- | ------------- | ------- | -------------- |
| USA     | 5 million DAU     | 1 GiB         | Snappy  | Tab Delim Gzip |
| Other   | 1 million DAU     | 1 GiB         | Snappy  | Tab Delim Gzip |

- Downloading from Onemata requires the requester to pay network traffic/trasfer costs

Onemata will provision access to AWS S3 Buckets and provide the S3 URI and paths.

Generally, Onemata uses AWS region `us-west-2` (Oregon) and names S3 Buckets using the following format:

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

> Access Policy Example:
> ```
> {
>     "Version": "2012-10-17",
>     "Statement": [
>         {
>             "Effect": "Allow",
>             "Action": [
>                 "s3:GetObject"
>             ],
>             "Resource": "arn:aws:s3:::AccountABucketName/*"
> 
>         }
>     ]
> }
> ```
Replace **AccountABucketName** with the bucket provided by Onemata. 


![](https://www.onemata.com/hs-fs/hubfs/Logos/Onemata%20Logo%20-%20wide.png)

### Onemata Standard Feed Buckets
1. s3://mobilelocationfeed.parquet.usw2.onemata.com/ Parquet version located in us-west-2
2. s3://mobilelocationfeed.csv.usw2.onemata.com/ Tab delimited version located in us-west-2

### Support or Contact

[Contact support](https://www.onemata.com/contact)
