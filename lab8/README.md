# LAB 8: Envoyer un email avec lambda en utilisant une queue SQS

## Méthode:

En repartant du lab6, modifiez la fonction pour qu'elle envoie des emails (attention au droits IAM de votre lambda). 
Partez d'un message ressemblant à ceci:  
  
```

{
  sender: "myemail@me.com",
  userEmail: "toto@test.com",
  userName: "me",
  subject: "demo SQS"
}

```

Une fois notre stack déployée :  
  
```bash

aws --profile <my profile> \   
    --region us-east-1 \    
    cloudformation deploy \           
    --template-file ./sqs.yml \          
    --stack-name <stack-name>  \        
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --parameter-overrides QueueName=<topic name>
    
```

Nous pouvons déclencher notre lambda en envoyant un message.  
  
```bash

aws --profile <profile> --region us-east-1 sqs send-message --queue-url <sqs url output> --message-body file://ressources/message.json

```