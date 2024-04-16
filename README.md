# Amazon S3 Versioning and Replication

## Purpose

**This mini project aims to explore Amazon S3 versioning and replication features.** You will create an S3 bucket, enable versioning, configure cross-region replication, and test these functionalities.

## Objectives

1. **Create an S3 Bucket**:
   - Learn to create an S3 bucket in the AWS Management Console.
2. **Enable Versioning**:
   - Understand how to enable versioning for an S3 bucket.
3. **Configure Cross-Region Replication**:
   - Set up replication to replicate objects to a different region.
4. **Test Versioning**:
   - Upload and modify objects to understand versioning behavior.
5. **Test Replication**:
   - Verify that objects are successfully replicated to the destination region.

## S3 Bucket Details

**Source Bucket**:

- Bucket Name: sourcebin
- Region: us-west-2

**Destination Bucket**:

- Bucket Name: destinationbin
- Region: us-west-1

### Steps

Let's delve into the fascinating world of **Amazon S3 versioning and replication**. Buckle up, and we'll guide you through the steps.

## Step 1: Creating S3 Buckets

### 1. Create the Source Bucket

1. Log in to your **AWS Management Console**.
2. Navigate to the **S3 service**.
3. Click on **"Create bucket"**.
4. Enter a unique bucket name (e.g., `sourcebin`) and choose the region (e.g., `us-west-2`) for the bucket.
5. Click on **"Create"** to create the bucket.

### 2. Create the Destination Bucket

1. Similarly, create a destination bucket (e.g., `destinationbin`) in the **AWS Region** where you want your data to be replicated.
2. Ensure the destination bucket is in a different region than the source bucket.

## Step 2: Enable Versioning

1. Enable Versioning for the **Source Bucket**:
   - Select the source bucket (`sourcebin`) from the **S3 dashboard**.
   - Go to the **"Properties"** tab.
   - Click on **"Versioning"** and then enable versioning for the bucket.
   - This allows storing multiple versions of an object in the bucket.

## Step 3: Configure Cross-Region Replication

### 1. Create IAM Roles for Cross-Region Replication

1. Navigate to the **IAM service** in the **AWS Management Console**.
2. Click on **"Roles"** in the left-hand navigation pane.
3. Click on **"Create role"** to create a new IAM role.
4. Choose **"AWS service"** as the trusted entity and select **"S3"** as the service that will use this role.
5. Attach a policy with permissions for cross-region replication (e.g., `AmazonS3FullAccess`) or create a custom policy with the required permissions.

   It's essential to ensure that your IAM policies are correctly configured for cross-region replication in Amazon S3. Here's the custom IAM policy you can include:

   ```json
   {
   	"Version": "2012-10-17",
   	"Statement": [
   		{
   			"Effect": "Allow",
   			"Action": [
   				"s3:GetReplicationConfiguration",
   				"s3:ListBucket",
   				"s3:GetObjectVersion",
   				"s3:GetObjectVersionTagging",
   				"s3:GetObjectVersionAcl",
   				"s3:GetObjectVersionForReplication",
   				"s3:ReplicateObject",
   				"s3:ReplicateDelete",
   				"s3:ReplicateTags",
   				"s3:ListBucketVersions"
   			],
   			"Resource": [
   				"arn:aws:s3:::source-bucket-name",
   				"arn:aws:s3:::source-bucket-name/*",
   				"arn:aws:s3:::destination-bucket-name",
   				"arn:aws:s3:::destination-bucket-name/*"
   			]
   		}
   	]
   }
   ```

   In this policy:

   - Replace `source-bucket-name` and `destination-bucket-name` with the actual names of your source and destination buckets.
   - The policy allows actions such as `s3:GetReplicationConfiguration`, `s3:ReplicateObject`, `s3:ReplicateDelete`, etc., which are necessary for cross-region replication.
   - The `Resource` section specifies the ARNs (Amazon Resource Names) of the source and destination buckets and their objects.

   After creating this custom policy, attach it to the IAM role you've set up for the source buckets during the cross-region replication setup process. This policy ensures that the IAM role have the required permissions to perform replication actions between the buckets.

6. Review and name the role (e.g., `S3CrossRegionReplicationRole`).
7. Create the role.

### 2. Configure Cross-Region Replication Rule

1. Go back to the **S3 dashboard** and select the **source bucket** (`sourcebin`).
2. In the **"Management"** tab, click on **"Replication Rules"** and then **"Create replication Rule."**
3. Input the replication rule name (e.g., `S3CrossRegionReplicationRule`) and ensure it is enabled.
4. Under the rule scope, apply it to **all objects** in the bucket.
5. Choose the **destination bucket** (`destinationbin`) in a different region and configure replication options (replicate all objects, deletions, encryption, etc.). This is optional and you have to be careful about not to incure addtitional costs. So it is better to leave all the replication options as default.
6. Select the **IAM role** created earlier (`S3CrossRegionReplicationRole`) or choose `create new role`.
7. Save the replication rule.

## Testing and Observations

1. **Testing Versioning Functionality**:

   - Upload a file to the source bucket.
   - Make changes to the file and upload it again with the same name.
   - Check the versions of the file in the S3 console under **"Versioning"** to verify version history.

2. **Testing Replication Functionality**:
   - Verify that objects are replicated to the destination bucket in the specified region.
   - Make changes to an object in the source bucket and ensure changes are replicated to the destination bucket.

## Observations and Challenges

- **Observations**:

  - Versioning helps in tracking changes made to objects over time.
  - Cross-region replication improves data durability by storing copies of data in different regions.

- **Challenges Faced**:
  - Understanding the configuration settings for cross-region replication initially posed a challenge.
  - Monitoring the replication process required additional attention to ensure it was functioning correctly.

Importance of S3 Versioning and Cross-Region Replication:

- **S3 Versioning**:

  - Maintains object history and provides protection against accidental deletion or modification of objects.
  - Facilitates easy recovery of previous versions of objects if needed.

- **Cross-Region Replication**:
  - Enhances data resilience by creating redundant copies of data in different geographic locations.
  - Improves disaster recovery capabilities by ensuring data availability even if one region experiences downtime.

By combining versioning and cross-region replication, organizations can significantly improve their data management practices, increase data durability, and strengthen their disaster recovery strategies. 🌟🔗
