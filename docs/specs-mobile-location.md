## Mobile Location Data Feed
`ONE-SALES-JA-21001` Updated: 2021-Feb-10

### Overview

The Onemata Mobile Location Feed is scalable, high-fidelity device location data. With mobile location data you can gain a competitive advantage by understanding robust real world movement data that resolves devices to audiences. Device identifier and type is combined daily with latitude, longitude and timestamp to provide real world human movement. Depending on the methodology various levels of accuracy are reported. Additional data that is included when available is: Device OS, User Agent, IP Address (v4 and v6), direction, and speed. 

### Column Field Information

| Column Name          | Description                                                                                                                                                                                              | Example #1                                                                                                                           | Example #2                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------- |
| mobile\_ad\_id       | Mobile Advertising ID                                                                                                                                                                                    | 188CF95F-6AD6-4394-AB09-68B7B61B1829                                                                                                 | 4D42B86E-44BA-4DE6-B9EC-FE18774A2DFA      |
| mobile\_ad\_id\_type | Android platform identifies as AAID and Apple platform as IDFA                                                                                                                                           | AAID                                                                                                                                 | IDFA                                      |
| utc\_timestamp       | This field includes the UTC timestamp reported when the device location was observed.                                                                                                                    | 1604959920                                                                                                                           | 1604965286                                |
| country              | This field shows the two character country in which the event occurred.                                                                                                                                  | IT                                                                                                                                   | GB                                        |
| latitude             | When reported with the observation the latitude in decimal degrees (DD) geographic coordinates. Positive latitudes are north of the equator, negative latitudes are south of the equator.                | 41.902782                                                                                                                            | 53.483959                                 |
| longitude            | When reported with the observation the longitude in decimal degrees (DD) geographic coordinates. Positive longitudes are east of the Prime Meridian; negative longitudes are west of the Prime Meridian. | 12.496366                                                                                                                            | \-2.244644                                |
| horizontal\_accuracy | This field shows the horizontal accuracy associated with the GPS location of the mobile device in meters.                                                                                                | 13.6                                                                                                                                 |                                           |
| heading              | This field shows the decimal degrees from north, clockwise 0 to 360 degrees.                                                                                                                             | 0                                                                                                                                    | 331.5234375                               |
| speed                | This field shows the speed of movement in meters/second based on GPS signal detected during the event.                                                                                                   | 0                                                                                                                                    | 32.38                                     |
| altitude             | Altitude associated with the GPS location of the mobile device in meters                                                                                                                                 | 66.2999954                                                                                                                           |                                           |
| vertical\_accuracy   | Vertical accuracy associated with the GPS location of the mobile device in meters.                                                                                                                       | 1.7                                                                                                                                  |                                           |
| device\_os           | Represents the device operating system and version when available.                                                                                                                                       | Android 4                                                                                                                            |                                           |
| user\_agent          | This field includes a description of the web user agent details.                                                                                                                                         | Mozilla/5.0 (Linux; U; Android 4.1.2; it-it; GT-N5100 Build/JZO54K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Safari/534.30 | 13.3.1                                    |
| ipv\_4               | This field shows the IPv4 Address detected during the event.                                                                                                                                             | 79.143.114.6                                                                                                                         | 77.103.41.101                             |
| ipv\_6               | This field shows the IPv6 Address detected during the event.                                                                                                                                             | FE80::5E3C:27FF:FE3C:52BF                                                                                                            |                                           |
| publisher            | An ID representing the publisher / app / app bundle                                                                                                                                                      | 151e0498a48d6160ad9c5d6e6bafd490                                                                                                     | f7cceb4540d611ea9d900e6b4470659a          |
| consent\_string      | IAB CCPA Framework (Framework) / (USP String) or the IAB’s GDPR solution (the Transparency and Consent Framework (TCF)                                                                                   | 11111110111110010100110011011111001010110                                                                                            | 11111110111110010100110011011111001011111 |

### Format & Delivery 

- Available daily on Amazon Web Services (AWS) Simple Storage Service (S3) around 2AM Eastern Timezone USA

| Country | Minimum DAU Order | Max File Size | Parquet | CSV            |
| ------- | ----------------- | ------------- | ------- | -------------- |
| USA     | 5 million DAU     | 1 GiB         | Snappy  | Tab Delim Gzip |
| Other   | 1 million DAU     | 1 GiB         | Snappy  | Tab Delim Gzip |

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


![](https://www.onemata.com/hs-fs/hubfs/Logos/Onemata%20Logo%20-%20wide.png)


### Support or Contact

[Contact support](https://www.onemata.com/contact)
