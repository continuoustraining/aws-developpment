# LAB 6: Lamdba déclenchée par un message SNS

## Méthode:

Nous allons maintenant corser un peu le jeu et commencer à chercher dans la documentation pour effectuer cette tache.  
  
Une fois notre stack déployée :  
  
```bash

aws --profile <my profile>  \   
    --region us-east-1       
    cloudformation deploy           
    --template-file ./sns.yml          
    --stack-name <stack-name>          
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND 
    --parameter-overrides TopicName=<topic name>
    
```

Nous pouvons déclencher notre lambda en envoyant un message.  
  
```bash

aws sns --profile <my profile> --region us-east-1 publish --topic-arn <my arn> --message "new message"


```