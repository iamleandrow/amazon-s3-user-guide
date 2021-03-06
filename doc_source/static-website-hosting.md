# How Do I Configure an S3 Bucket for Static Website Hosting?<a name="static-website-hosting"></a>

You can host a static website on Amazon S3\. On a static website, individual webpages include static content\. A static website might also contain client\-side scripts\. By contrast, a dynamic website relies on server\-side processing, including server\-side scripts such as PHP, JSP, or ASP\.NET\. Amazon S3 does not support server\-side scripting\.

You can use the following quick procedures to configure an S3 bucket for static website hosting in the Amazon S3 console\. For more information and detailed walkthroughs, see [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the *Amazon Simple Storage Service Developer Guide*\.

**Topics**
+ [Step One: Configure a Amazon S3 Bucket for Static Website Hosting](#configure-bucket-website-hosting)
+ [Step Two: Set Permissions for Static Website Access](#set-permissions-static-website-access)
+ [Step Three: Test Your Website Endpoint](#test-your-website-endpoint)

## Step One: Configure a Amazon S3 Bucket for Static Website Hosting<a name="configure-bucket-website-hosting"></a>

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the **Bucket name** list, choose the name of the bucket that you want to enable static website hosting for\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/choose-bucket-name.png)

1. Choose **Properties**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/choose-properties-tab.png)

1. Choose **Static website hosting**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/static-website-hosting-box.png)

1. Choose **Use this bucket to host a website**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/website-endpoint.png)

1. In **Index document**, enter the name of the index document, typically `index.html`\. 

   When you configure a bucket for website hosting, you must specify an index document\. Amazon S3 returns this index document when requests are made to the root domain or any of the subfolders\. For more information, see [Configuring Index Document Support](https://docs.aws.amazon.com/AmazonS3/latest/dev/IndexDocumentSupport.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. \(Optional\) If you want to provide your own custom error document for 4XX class errors, in **Error Document**, enter the custom error document filename\. 

   If you do not specify a custom error document and an error occurs, Amazon S3 returns a default HTML error document\. For more information, see [Custom Error Document Support](https://docs.aws.amazon.com/AmazonS3/latest/dev/CustomErrorDocSupport.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. \(Optional\) If you want to specify advanced redirection rules, in the **Edit redirection rules** box, enter XML to describe the rules\.

   For example, you can conditionally route requests according to specific object key names or prefixes in the request\. For more information, see [Advanced Conditional Redirects](https://docs.aws.amazon.com/AmazonS3/latest/dev/how-to-page-redirect.html#advanced-conditional-redirects) in the *Amazon Simple Storage Service Developer Guide*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/static-website-hosting-enable.png)

1. Choose **Save**\.

1. Upload the index document to your bucket\.

   For step\-by\-step instructions on uploading an object to an S3 bucket, see [Uploading Files by Pointing and Clicking](upload-objects.md#upload-objects-by-file-selection)\. 

1. Upload other files for your website, including optional custom error documents\.

In the next section, you set the permissions required to access your bucket as a static website\.\.

## Step Two: Set Permissions for Static Website Access<a name="set-permissions-static-website-access"></a>

After you enable website access for your bucket, you must set permissions for static website access, which involves editing your Amazon S3 Block Public Access settings and adding a bucket policy that grants pubic read access\. For more information, see [Permissions Required for Website Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html) in the *Amazon Simple Storage Service Developer Guide*\.

By default, Amazon S3 does not allow public access to your account or buckets\. Before you edit S3 Block Public Access settings, confirm that you want anyone on the internet to be able to access your bucket\. For more information, see [Using Amazon S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html) in the *Amazon Simple Storage Service Developer Guide*\.

**To set permissions required for website access**

1. Select the bucket, and choose **Edit public access settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/edit-public-access-settings.png)

1. Clear **Block *all* public access**, and choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/edit-public-access-clear.png)

1. To confirm your changes, enter **confirm**, and then choose **Confirm**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/images/edit-public-access-confirm.png)

1. To confirm that objects in the bucket can now be publicly readable, do the following:

   1. Expand the side navigation pane, and choose **Buckets**\.

   1. Under **S3 buckets**, find your bucket and look under **Access**\.

      Under **Access**, you see **Objects can be public**\. 

      If the **Access** is **Bucket and objects not public**, you might have to edit your account\-level S3 Block Public Access settings before you can add the bucket policy\. For more information, see [How Do I Edit Public Access Settings for All the S3 Buckets in an AWS Account?](block-public-access-account.md)

1. Under **S3 buckets**, choose your bucket name\.

1. Choose **Permissions**\.

1. Choose **Bucket Policy**\.

1. To grant public read access for your website, copy the following bucket policy, and paste it in the **Bucket policy editor**\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": [
                   "s3:GetObject"
               ],
               "Resource": [
                   "arn:aws:s3:::example-bucket/*"
               ]
           }
       ]
   }
   ```

1. Update the `Resource` to include your bucket name\.

   In the above example bucket policy, *example\-bucket* is the bucket name\. To use this bucket policy with your own bucket, you must update this name to match your bucket name\. For example, if your bucket name is `my-example-bucket`, the bucket policy would look like this:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": [
                   "s3:GetObject"
               ],
               "Resource": [
                   "arn:aws:s3:::my-example-bucket/*"
               ]
           }
       ]
   }
   ```

1. Choose **Save**\.

   A warning appears indicating that the bucket has public access\. On **Bucket Policy**, you see a **Public** label\.

   If you see an error that says `Policy has invalid resource`, confirm the bucket name in the bucket policy matches your bucket name\. For information about adding a bucket policy, see [How Do I Add an S3 Bucket Policy?](add-bucket-policy.md)

   If the **Bucket policy editor** does not allow you to save the bucket policy, check your account\-level and bucket\-level block public access settings\. For more information about website permissions, see [Permissions Required for Website Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Step Three: Test Your Website Endpoint<a name="test-your-website-endpoint"></a>

Once you configure your bucket as a static website and set permissions, you can access your website through an Amazon S3 website endpoint\. For more information, see [Website Endpoints](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteEndpoints.html) in the *Amazon Simple Storage Service Developer Guide*\. For a complete list of Amazon S3 website endpoints, see [Amazon S3 Website Endpoints](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_website_region_endpoints) in the *Amazon Web Services General Reference*\.