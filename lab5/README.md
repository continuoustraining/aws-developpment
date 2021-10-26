# LAB 5: Lamdba déclenchée par un nouveau fichier sur S3.

## Méthode:

Le template qui vous ai proposé contient l'ensemble du coudformation.  
A vous de créer la fonction lambda. Le template vous propose de le faire en python mais libre à vous de choisir votre langage favori.
  
Une fois votre code déployer:  

```bash

aws --profile <my profile>  \   
    --region us-east-1 \    
    cloudformation deploy  \         
    --template-file ./event-lambda.yml          
    --stack-name <my stack name>           
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND 
    --parameter-overrides BucketName=<my bucket>

```

vous pouvez envoyer sur votre S3 le fichier CSV d'exemple.  
  
```

aws s3 --profile <my profile> --region us-east-1 sync ressources/ s3://<my bucket>

```