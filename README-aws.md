# Configuring buckets for AWS S3

In general, we will configure two types of buckets:

* One or more private buckets to be used for storing badges, grades, videos and Open Response Assessment (aka ORA2) files. `S3_STORAGE_BUCKET`, `S3_FILE_UPLOAD_BUCKET`, and `S3_GRADE_BUCKET` should be set to use these types of buckets.
* A public bucket, used only for `S3_PROFILE_IMAGE_BUCKET` to contain profile images.

## Private Bucket

### Bucket creation

Create a new bucket in the AWS S3 console with the settings below:

1. `Bucket name`: my-openedx-bucket-name
2. `AWS Region`: no need to change
3. `Object Ownership`: ACLs disabled (recommended)
4. `Block Public Access settings for this bucket`: Block all public access
5. `Bucket Versioning`: Disable
6. `Tags`: It is optional
7. `Default encryption`: Disable
8. Then click on: **Create bucket**

### Bucket Permissions

`tutor-contrib-s3` will use pre-signed URLs to communicate with private buckets.
Therefore we need to configure the bucket to allow Open edX to access (`GET`) and upload (`PUT`) the files only from the domain your Open edX instance is hosted on.

Access the Bucket Permissions tab and change the setting below (make sure values `LMS_HOST` and `CMS_HOST` are replaced with correct URLs.):

**Cross-origin resource sharing** (CORS):

```json
[
    {
        "AllowedHeaders": [
            "Content-disposition",
            "Content-type",
            "X-CSRFToken"
        ],
        "AllowedMethods": [
            "GET",
            "PUT"
        ],
        "AllowedOrigins": [
            "https://${LMS_HOST}",
            "https://${CMS_HOST}"
        ],
        "ExposeHeaders": [],
        "MaxAgeSeconds": 3000
    }
]
```

## Profile Pictures Bucket

Open edX will store user profile images in this bucket. These files are accessed by a simple URL without any authentication. Here is an example of such a URL:
`https://my-openedx-profile-images.s3.amazonaws.com/openedx/media/profile-images/1c7864143410dfadbbc267fe4593806a_50.jpg?v=1650476461`

### Bucket creation

Create a new bucket in the AWS S3 console with the settings below:

1. `Bucket name`: openedx-profile-pictures-bucket
2. `AWS Region`: no need to change
3. `Object Ownership`: ACLs disabled (recommended)
4. `Block Public Access settings for this bucket`: Uncheck "Block all public access"
5. `Bucket Versioning`: Disable
6. `Tags`: It is optional
7. `Default encryption`: Disable
8. Then click: **Create bucket**

### Bucket Permissions

We need to configure the bucket to allow Open edX to access (`GET`) the files using a simple URL but only from the domain your Open edX instance is hosted on.
Also give full access (read, write, delete, list) to the backend user account (authenticated with `OPENEDX_AWS_ACCESS_KEY` and `OPENEDX_AWS_SECRET_ACCESS_KEY`).

Access the Bucket Permissions tab and change the configurations below:

**Bucket policy**:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "openedxWebAccess",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject*",
            "Resource": "arn:aws:s3:::${AWS_PROFILE_BUCKET_NAME}/*"
        },
        {
            "Sid": "openedxBackendAccess",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::${AWS_ACCOUNT_ID}:user/${AWS_IAM_USER}"
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::${AWS_PROFILE_BUCKET_NAME}/*"
        }
    ]
}
```

**Note**: Make sure you replace `AWS_PROFILE_BUCKET_NAME`, `AWS_ACCOUNT_ID` and `AWS_IAM_USER` with correct values.

**Cross-origin resource sharing** (CORS):

```json
[
    {
        "AllowedHeaders": [],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "https://${LMS_HOST}",
            "https://${CMS_HOST}"
        ],
        "ExposeHeaders": [],
        "MaxAgeSeconds": 3000
    }
]
```

**Note**: Replace `LMS_HOST` and `CMS_HOST` with URLs of your LMS and CMS.

## Examples of variable values

* `LMS_HOST`: openedx.com
* `CMS_HOST`: studio.openedx.com
* `AWS_PROFILE_BUCKET_NAME`: openedx-profile-pictures-bucket
* `AWS_ACCOUNT_ID`: 123456789012
* `AWS_IAM_USER`: openedx-user

## CloudFront

If you need to accelerate website content delivery, another option to serve the profile pictures is to use [CloudFront](https://aws.amazon.com/cloudfront/) in front of AWS S3.

Amazon CloudFront is a fast content delivery network ([CDN](https://en.wikipedia.org/wiki/Content_delivery_network)) service that securely delivers data to customers globally with low latency and high transfer speeds.

CloudFront caches the data and sends it to the user from the nearest edge location, instead of picking it from the original location (S3 Bucket).
This speeds up the transfer rate.

For more information, follow [this guide](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/).

If you use this alternative, you must set `S3_PROFILE_IMAGE_CUSTOM_DOMAIN` to your CloudFront distribution domain name, e.g.: `d31h0g0s5c5qjp.cloudfront.net`.