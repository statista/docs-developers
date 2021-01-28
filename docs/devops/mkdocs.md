#mkdocs

##1. General
[mkdocs Getting Started](https://www.mkdocs.org/#getting-started "mkdocs Getting Started")

##2. Installation
```
yum install mkdocs
pip install mkdocs
```
* create mkdocs folder structure
    ```
    documents
    -- mkdocs.yml
    -- docs
    -- index.md
    -- faq.md
    -- images/
    -- stylesheets/
    ```
* Install material theme for mkdocs 
`pip install mkdocs-material`

* Setup mkdocs by updating mkdocs.yml
    ```
    site_name: STAT4.0 docs
    site_description: Statista 4.0 documentation
    theme:
    name: 'material'
    favicon: 'https://cdn.statcdn.com/static/favicon.ico'
    logo: 'https://cdn.statcdn.com/static/img/Statista-Logo-Color-Primary.png'
    extra_css:
    - 'stylesheets/extra.css'
    ```
* add _extra.css_ if needed and favicon (`wget de.statista.com/favicon.ico`)

* Serve site locally
`mkdocs serve` and test if everything works

##S3 Hosting SETUP
_**prerequisite**_: [configure AWS-CLI](aws_cli)

* Create S3 bucket & subdomain with [Cloudformation Template](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-s3.html)

   		AWSTemplateFormatVersion: 2010-09-09
   		Description: Creates a public s3 bucket for html hosting, subdomain and https redirection

   		# ---------- Parameters -------------------------------------------------------
   		Parameters:
   		  CertificateArn:
   			Type: String
   			Default: <some ACM certificate ARN>
   			Description: Certificate must be created before CloudFormation stack so the value is fixed
   		  BucketName:
   			Type: String
   			Default: bw-hosting-https-aws
   		  DomainName:
   			Type: String
   			Default: app.example.com
   		  RootDomainName:
   			Type: String
   			Default: example.com.

   		# ---------- Resources lists --------------------------------------------------
   		Resources:
   		  S3Hosting:
   			Type: AWS::S3::Bucket
   			Properties:
   			  BucketName: !Ref BucketName

   		  CDNOriginIdentity:
   			Type: AWS::CloudFront::CloudFrontOriginAccessIdentity
   			Properties:
   			  CloudFrontOriginAccessIdentityConfig:
   				Comment: !Sub "Cloudfront Origin identity for ${DomainName}"

   		  CDN:
   			Type: "AWS::CloudFront::Distribution"
   			Properties:
   			  DistributionConfig:
   				Aliases: 
   				  - !Ref DomainName
   				DefaultCacheBehavior:
   				  AllowedMethods:
   					- GET
   					- HEAD
   				  CachedMethods:
   					- GET
   					- HEAD
   				  ForwardedValues:
   					QueryString: True
   				  TargetOriginId: !Sub "S3-origin-${S3Hosting}"
   				  ViewerProtocolPolicy: redirect-to-https
   				DefaultRootObject: index.html
   				Enabled: True
   				HttpVersion: http2
   				IPV6Enabled: True
   				Origins:
   				  - DomainName: !GetAtt S3Hosting.RegionalDomainName
   					Id: !Sub "S3-origin-${S3Hosting}"
   					S3OriginConfig:
   					  OriginAccessIdentity: !Sub "origin-access-identity/cloudfront/${CDNOriginIdentity}"
   				PriceClass: PriceClass_100 # PriceClass_100 / PriceClass_200 / PriceClass_All
   				ViewerCertificate:
   				  AcmCertificateArn: !Ref CertificateArn
   				  MinimumProtocolVersion: TLSv1.2_2018
   				  SslSupportMethod: sni-only

   		  S3HostingBucketPolicy:
   			Type: AWS::S3::BucketPolicy
   			Properties:
   			  Bucket: !Ref S3Hosting
   			  PolicyDocument:
   				Statement:
   				  - Action:
   					  - "s3:GetObject"
   					Effect: Allow
   					Principal:
   					  AWS: !Sub "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity ${CDNOriginIdentity}"
   					Resource: !Sub "arn:aws:s3:::${S3Hosting}/*"
   				Version: "2012-10-17"

   		  DNS:
   			Type: AWS::Route53::RecordSetGroup
   			Properties:
   			  HostedZoneName: !Ref RootDomainName
   			  RecordSets:
   				- AliasTarget:
   					DNSName: !GetAtt CDN.DomainName
   					HostedZoneId: Z2FDTNDATAQYW2 # CloudFront hosted zone ID is fixed
   				  Name: !Ref DomainName
   				  Type: A


* [Fix Cloudfront Subdirectories](https://medium.com/radon-dev/redirection-on-cloudfront-with-lambda-edge-e72fd633603e "Source")

* Create Lambda Function _cloudfrontMkdocsSubdirectoryHandler_
    * Edit IAM Permissions -> [add trust relationship](https://stackoverflow.com/questions/53796032/cannot-create-aws-lamda-function-due-to-some-cryptic-error-message)
    * edit function
  
    ```
  const path = require('path');
    
    exports.handler = (event, context, callback) => {
      const { request } = event.Records[0].cf;
      
      console.log('Request URI: ', request.uri);

      const parsedPath = path.parse(request.uri);
      let newUri;

      console.log('Parsed Path: ', parsedPath);
      
      if (parsedPath.ext === '') {
        newUri = path.join(parsedPath.dir, parsedPath.base, 'index.html');
      } else {
        newUri = request.uri;
      }

      console.log('New URI: ', newUri);

      // Replace the received URI with the URI that includes the index page
      request.uri = newUri;
      
      // Return to CloudFront
      return callback(null, request);
    };
  ```
  
* add Trigger - select Cloudfront Distribution - deploy to Lambda@Edge
* Lambda Invalidate
    * IAM Rechte CodePipeline + Lambda CloudFront
* Build HTML
    ```
    mkdocs build
    ```

* Copy Files to S3
    ```
    aws s3 sync ./site s3://stat-docs-developers
    ```
### Links
* [mkdocs-Codepipeline-to-S3](https://github.com/aurbac/mkdocs-codepipeline-github-to-s3)
* [automated Build](https://medium.com/taptuit/automated-build-deploy-with-aws-codepipeline-f0714d62f61c)