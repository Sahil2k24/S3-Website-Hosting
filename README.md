# Configuring a Static Website with Amazon S3 and CloudFront

This guide walks you through the steps to set up a static website using Amazon S3 and CloudFront. By following these steps, you will be able to host your static website securely and efficiently on AWS.

## Step 1: Create an S3 Bucket

1. Sign in to the AWS Management Console.
2. Make sure you are in the US-WEST-2 (Oregon) region.
3. Navigate to the S3 service.
4. Click on "Create bucket".
5. Provide a unique name for your bucket and select the Oregon region.
6. Enable ACLs (Access Control Lists).
7. Uncheck the "Block all public access" option and acknowledge the warning that appears.
8. Click "Create bucket" to finalize the bucket creation.

## Step 2: Set Bucket Policy

1. In the S3 dashboard, click on the name of your newly created bucket.
2. Go to the "Permissions" tab.
3. Scroll to the "Bucket policy" section and click "Edit".
4. Copy and paste the following policy, replacing `YOUR_BUCKET_NAME` with the name of your S3 bucket:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "AddPerm",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
      ]
    }
    ```

5. Click "Save changes" to apply the policy.

## Step 3: Enable Static Website Hosting

1. Go to the "Properties" tab of your S3 bucket.
2. Scroll down to the "Static website hosting" section and click "Edit".
3. Select "Enable" and then choose "Host a static website".
4. Enter `index.html` as the Index document.
5. Optionally, enter an error document such as `error.html`.
6. Click "Save changes".

## Step 4: Upload Your Website Files

1. Go to the "Objects" tab and click "Upload".
2. Click "Add files" to upload your HTML, CSS, JavaScript, images, and other static website files.
3. Optionally, organize your files into folders such as CSS and assets.
4. Click "Upload" to complete the process.

## Step 5: Configure CloudFront Distribution

1. In the AWS Management Console, search for CloudFront and open the CloudFront service.
2. Click "Create a CloudFront Distribution".
3. Under the "Origin" settings, enter the S3 static website hosting endpoint you created earlier.
4. If your S3 bucket is not listed, manually enter `<name_of_the_bucket>.s3.amazonaws.com`.
5. Under "Origin access", select "Origin access control settings" and click "Create new OAC".
6. Ensure "Sign requests" is selected and click "Create".
7. Leave other fields as default and set the "Default root object" to `index.html`.
8. Scroll down and select "Use only North America and Europe" in the Price class selection.
9. Click "Create distribution".

## Step 6: Update Bucket Policy for CloudFront

1. In the CloudFront distribution details page, click "Copy policy".
2. Return to the S3 bucket, go to the "Permissions" tab, and edit the "Bucket policy".
3. Replace the current policy with the copied CloudFront policy:

    ```json
    {
      "Version": "2008-10-17",
      "Id": "PolicyForCloudFrontPrivateContent",
      "Statement": [
        {
          "Sid": "AllowCloudFrontServicePrincipal",
          "Effect": "Allow",
          "Principal": {
            "Service": "cloudfront.amazonaws.com"
          },
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
          "Condition": {
            "StringEquals": {
              "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_DISTRIBUTION_ID"
            }
          }
        }
      ]
    }
    ```

4. Click "Save changes".

## Step 7: Block Public Access to S3 Bucket

1. In the "Permissions" tab of your S3 bucket, click "Edit" under the "Block public access (bucket settings)" section.
2. Select "Block all public access" and click "Save changes".
3. Confirm by typing "confirm" and clicking "Confirm".

## Step 8: Access Your Website

1. Return to the CloudFront distributions table.
2. Copy the domain name of your CloudFront distribution.
3. Paste this domain name into the address bar of a new browser tab.
4. Your static website should now be accessible via the CloudFront distribution URL.

---

By following these steps, you can leverage AWS's robust infrastructure to host your static website with global reach and enhanced security. If you encounter any issues or have questions, feel free to reach out.

Happy hosting!

#AWS #CloudFront #S3 #StaticWebsiteHosting #WebDevelopment #CloudComputing
