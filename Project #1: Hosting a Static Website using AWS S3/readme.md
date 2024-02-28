Project Introduction
I am creating a static website using AWS S3 service (Simple Storage Service).
Normally access to s3 is via AWS API’s or CLI. Static website hosting allows access via HTTP e.g. Blogs.
An endpoint is created and the DNS name of the website is influenced based on the bucket name.
You can use a custom domain using Route 53 but the bucket name should be the same name as the domain name. So make sure you reserve the bucket name if you plan to use a custom domain. For e.g. If the domain name is sruthianem.io then the bucket name should also be sruthianem.io.
Static website hosting can be used for:
- Offloading storage to s3 from compute instances
- Out of band pages e.g. Maintenance pages during scheduled maintenance
The website contains 2 static content pages
- Index.html (Default)
- Error.html
Steps & Workflow
Create a bucket
Enable static website hosting
Upload objects into the bucket
Attach a bucket policy
Access the static website
Demo
1. Creating a bucket

Since the website should be accessible via HTTP we should disable the Block all public access checkbox. Unchecking the box means you will be able to grant public access. It does not mean the public access is granted automatically. S3 is private by default.


Everything else can be left as default and click on Create bucket


The bucket has been created


2. Enable Static website hosting
Click on the bucket and access the properties tab. We need to enable Static website hosting. By Default it is disabled:


Enter the index.html & error.html documents



3. Upload Objects into the bucket
Upload the web pages index.html & error.html
Add any supporting files (e.g. images)

4. Attach bucket policy
S3 by default is private. Any anonymous or unauthenticated user will NOT be able to access the bucket & its objects. The solution is to attach a bucket policy.

{
    "Version":"2012-10-17",
    "Statement":[
      {
        "Sid":"PublicRead",
        "Effect":"Allow",
        "Principal": "*",
        "Action":["s3:GetObject"],
        "Resource":["arn:aws:s3:::sruthianem-03111989/*"]
      }
    ]
  }
This bucket policy allow all principals to perform GetObject action on the bucket.

Click on permissions tab and paste the code under the bucket policy section:


5. Access the static website
To access the website, go to the static website hosting section under the properties tab and click on the DNS name. This is the endpoint that is derived based on the name of the bucket.


The page is now accessible:


If you specify an object that does not exist, instead of index.html the system will load error.html.

