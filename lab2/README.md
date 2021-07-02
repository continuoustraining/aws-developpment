# LAB 2: créer des bucket S3

## méthode:

Sur base du template fourni, dans la section "ressources", ajoutez votre bucket Privé.  
  
```

  S3PrivateBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref PrivateBucketName
      
```

Ajoutez ensuite votre bucket public.  

```

  S3PublicBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref PublicBucketName
      AccessControl: PublicRead
      WebsiteConfiguration:
        IndexDocument: index.html
        ErrorDocument: error.html
    DeletionPolicy: Retain
    
```

Il nous manque la policy pour les objets du buckets public:  
  
```

  BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      PolicyDocument:
        Id: MyPolicy
        Version: 2012-10-17
        Statement:
          - Sid: PublicReadForGetBucketObjects
            Effect: Allow
            Principal: '*'
            Action: 's3:GetObject'
            Resource: !Join 
              - ''
              - - 'arn:aws:s3:::'
                - !Ref S3PublicBucket
                - /*
      Bucket: !Ref S3PublicBucket

```

Dans la section "output", ajoutez les lignes suivantes :  

```

  PrivateBucket:
    Value: !Ref S3PrivateBucket
    Description: "Private bucket name"`

  PublicBucket:
    Value: !Ref S3PublicBucket
    Description: "Public bucket name"

  WebsiteURL:
    Value: !GetAtt 
      - S3PublicBucket
      - WebsiteURL
    Description: "URL for website hosted on S3"
    
  S3BucketSecureURL:
    Value: !Join 
      - ''
      - - 'https://'
        - !GetAtt 
          - S3PublicBucket
          - DomainName
    Description: "Name of S3 bucket to hold website content"

```

Nous allons maintenant lancer notre stack en ligne de commandes :  
Pensez à bien remplacer les paramètres :  
  
```bash

aws --profile <my profile> \
    --region us-east-1 \
      cloudformation deploy \
          --template-file ./s3.yml \
          --stack-name <my stack name> \
          --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
          --parameter-overrides \
            PublicBucketName=<bucket public> \
            PrivateBucketName=<bucket privé>

```

Nous allons maintenant regarder la sortie de notre stack:  

```bash

aws --profile <my profile> \
    --region us-east-1 \
      cloudformation describe-stacks --stack-name <my stack name> 

```

Il est temps de mettre notre contenu dans notre bucket :  

```bash

aws --profile <my profile> --region us-east-1 s3 sync ./ressources/ s3://<public bucket name> 

```