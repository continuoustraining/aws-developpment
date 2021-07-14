# LAB 7: Envoyer un email avec lambda

## Méthode:

En repartant du lab6, modifiez la fonction pour qu'elle envoie des emails (attention au droits IAM de votre lambda). 
Partez d'un message ressemblant à ceci:  
  
```

{
  sender: "myemail@me.com",
  userEmail: "toto@test.com",
  userName: "me",
  subject: "demo ses"
}

```

Une fois notre stack déployée :  
  
```bash

aws --profile <my profile> \   
    --region us-east-1 \    
    cloudformation deploy \           
    --template-file ./ses.yml \          
    --stack-name <stack-name>  \        
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --parameter-overrides TopicName=<topic name>
    
```

Nous pouvons déclencher notre lambda en envoyant un message.  
  
```bash

aws sns --profile <my profile> --region us-east-1 publish --topic-arn <my arn> --message file://ressources/message.json


```