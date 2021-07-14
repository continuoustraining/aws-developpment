# LAB 9: Mise en place d'une API Gateway

## Méthode:

Premièrement déployez la stack d'exemple:  
  
```bash

aws --profile <my profile> \   
    --region us-east-1 \    
    cloudformation deploy \           
    --template-file ./api-gateway.yml \          
    --stack-name <stack-name>  \        
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --parameter-overrides apiGatewayName=<gateway name> apiGatewayStageName=<gateway stage name> 

```

Vous pouvez ensuite tester votre méthode :  
  
```bash

curl --request POST https://<url>

```
  
Nous allons maintenant ajouter une méthode GET et une nouvelle lamdba.  