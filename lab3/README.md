# LAB 3: Ajouter un CDN cloudfront devant votre bucket S3 Public.

## Méthode:

Sur le base du template présent dans ce répertoire (similaire à celui obtenu après le lab 2),  
Ajoutez votre CDN :  
  
```

  WebsiteCDN:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Comment: CDN for S3-backed website
        Aliases:
        - !Ref DomainName
        HttpVersion: http2
        CustomErrorResponses:
        - ErrorCode: '404'
          ResponsePagePath: "/error.html"
          ResponseCode: '404'
          ErrorCachingMinTTL: '300'
        Enabled: 'true'
        DefaultCacheBehavior:
          ForwardedValues:
            QueryString: 'true'
          Compress: true
          TargetOriginId: only-origin
          ViewerProtocolPolicy: redirect-to-https
        DefaultRootObject: index.html
        Origins:
        - CustomOriginConfig:
            HTTPPort: '80'
            HTTPSPort: '443'
            OriginProtocolPolicy: http-only
          DomainName: !GetAtt S3PublicBucket.DomainName
          Id: only-origin
        PriceClass: PriceClass_100
        ViewerCertificate:
          AcmCertificateArn: !Ref CertificateArn
          SslSupportMethod: sni-only

```

Nous allons ajouter une entrée DNS qui sera un alias de notre distribution cloudfront.   

```

  WebsiteDNSName:
    Type: AWS::Route53::RecordSet
    Properties:
      HostedZoneName: !Join ['', [!Ref 'HostedZone', .]]
      Comment: CNAME redirect custom name to CloudFront distribution
      Name: !Ref DomainName
      Type: A
      AliasTarget:
        HostedZoneId: Z2FDTNDATAQYW2
        DNSName: !GetAtt WebsiteCDN.DomainName

```

Nous allons mettre à jour notre stack.  
Des paramêtres ont été ajoutés, les valeurs seront fourni par le formateur en fonction de votre environement.  
  
```bash

aws --profile <my profile> --region us-east-1  cloudformation deploy \          
--template-file ./cloudfront.yml. --stack-name <my stack> \           
--capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \    
--parameter-overrides PublicBucketName=<to replace>  \   
CertificateArn=<to replace>  \   
HostedZone=<to replace>   DomainName=<to replace> \     
PrivateBucketName=<to replace>

```
  
Maintenant vous pouvez voir votre site en action.  