# LAB 10: Mise en place d'un site dynamique avec lamdba@edge

## Méthode:

Premièrement déployez la stack d'exemple:  
  
```bash

aws --profile <my profile> \   
    --region us-east-1 \    
    cloudformation deploy \           
    --template-file ./lambdaedge.yml \          
    --stack-name <stack-name>  \        
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --parameter-overrides AcmCertificateArn=<arn> HostedZone=<training hosted zone> DomainName=<domain name>

```

Nous allons créer une retour en html.  
  
Vous pouvez l'afficher dans votre navigateur.  
  
Nous allons ajouter un retour 404 pour les images.  

Envoyez les resources dans votre bucket S3.  
  
Nous allons ajouter l'image dans la page.  
  
Nous allons maintenant pouvoir tester le résultat.