![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Simple Storage Service on Amazon Web Services setup

## Instructions

Fork and clone this repository.

Read over all the instructions before proceeding.

Follow the steps outlined to create and gain programatic access to an AWS S3
 bucket.

## Prerequisites

-   An `AWS` _(Amazon Web Services)_ account
-   A Credit card is required to verify your AWS account. 

If you do not have an account, open [AWS](https://aws.amazon.com/) and click
 `Sign In to the Console`.
Amazon provides a [free tier](http://aws.amazon.com/free/),
 with some limitations, for twelve months after you sign-up for an AWS account.

## Motivation

Storing large static files is a common need for a web application.
Accepting image uploads from authorized users but allowing public read access is
 a frequent example.

AWS provides a variety of APIs, one of which is easily used for this purpose.
This guide helps ensure access to these APIs is restricted.

Why is the important?

Using any metered API has financial risks.  Using many APIs may have data risks
 (information loss or exposure).

Using restrictive access control with AWS ensures that even if an identity is
 compromised, the actual risks, financial and otherwise, are limited.

## AWS S3 access control

1.  Open the [AWS Consle](https://console.aws.amazon.com/console/) in your
 browser
1.  From the `AWS` console open tabs for
 `IAM` _(Identity and Access Management)_ and `S3` _(Simple Storage Service)_.

### Identity and Access Management (IAM)

Identities are how we grant access to AWS APIs.

In the [IAM](https://console.aws.amazon.com/iam) tab:

1.  Select `Users` in the left sidebar.
1.  Click `Create New Users` near the top of the page.
1.  Enter `wdi-upload` into box `1.`.
1.  Make sure `Generate an access key for each user` is checked.
1.  Click `Create`.
1.  Click `Download Credentials`.
1.  Save the file `credentials.csv` to this repository.
1.  Click `Close`
1.  Click on the newly created user.
1.  Copy the `User ARN` _(Amazon Resource Name)_ and save it in [arn.txt](arn.txt).

We'll need the User ARN to grant access to an S3 bucket we'll use for uploads.
We'll also need an `Access Key` _(Access Key Id and Secret Access Key)_ for this
 IAM User to upload files via the S3 API.
The Access Key is contained in credentials.csv.

**Note well:** credentials.csv contains `secrets`!
Do not share them or store them in git.
The [.gitignore](.gitignore) in this repository explicitly ignores this file.

### Simple Storage Service (S3)

S3 stores files you upload in `buckets`.  A bucket is a top level namespace
 for your files.

In the [S3](https://console.aws.amazon.com/s3) tab:

1.  Click `Create Bucket`.
 This opens the `Create a Bucket - Select a Bucket Name and Region` modal.
1.  Enter a name in the `Bucket Name` box. It must be unique among all S3
 buckets.
1.  Select `US Standard` for the `Region`.
1.  Click `Create`.
1.  Make sure the bucket and `Properties` are selected.
1.  Open the `Permissions` dropdown in the right sidebar.
1.  Click `Add bucket policy` near the bottom of the `Permissions` dropdown.
1.  At the bottom of the `Bucket Policy Editor` modal,
 click `AWS Policy Generator`.  This opens the AWS Policy Generator page.
1.  On the AWS Policy Generator page

    1.  Step 1: Select Policy Type

        1.  For `Select Type of Policy` use `S3 Bucket Policy`.

    1.  Step 2: Add Statement(s)

        1.  Select `Allow` for `Effect`.
        1.  Paste the User ARN into the `Principal` box.
        1.  Select `PutObject` and `PutObjectAcl` for `Actions`.
        1.  Enter `arn:aws:s3:::<bucket_name>/*` into the
          `Amazon Resource Name (ARN)` box.
        1.  Click the `Add Statement`.

    1.  Step 3: Generate Policy

        1.  Click `Generate Policy`
        1.  Copy the JSON from the `Policy JSON Document` modal.

1.  Return to the S3 tab.
1.  Paste the bucket policy into the `Bucket Policy Editor` modal.
1.  Click `Save`.
1.  Click `Save` in the `Permissions` dropdown.

You have now created and granted access to an S3 bucket.

These steps limit upload access to one bucket for the identity `wdi-upload`.

This is one specific and restrictive way of implementing access control.
AWS provides many different mechanisms to grant and restrict access.

#### Example bucket policy JSON

```json
{
  "Version": "2012-10-17",
  "Id": "Policy1439826519004",
  "Statement": [
    {
      "Sid": "Stmt1439826516658",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<AWS Account Id>:user/<IAM User Name>"
      },
      "Action": [
        "s3:PutObjectAcl",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::<bucket_name>/<key_name>"
    }
  ]
}
```

## Checklist

-   [ ] Create (or select) an AWS Identity.
-   [ ] Create and download credentials for this identity.
-   [ ] Create an S3 bucket.
-   [ ] Create a bucket policy.

## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or alternative
licensing, please contact legal@ga.co.
