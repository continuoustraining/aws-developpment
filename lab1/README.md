# LAB 1: créer un utilisateur IAM via cloudfomration.

## méthode:

Dans la policy de l'utilisateur:  
```
              -
                Effect: Allow
                Action:
                  - iam:*
                  - s3:*
                Resource: '*'
```

ajoutez les droits suivants :
```
- lamdba:*
- ec2:*
- route53:*
- ecr:*
- ecs:*
- rds:*
- cloudfront:*
- sns:*
- sqs:*
- ses:*
```
  
Dans la console, recherchez le service cloudformation.  
Choisissez "create stack" et "With new ressources (standard)".  
Utilisez les options : "template is ready" et upload a template file.  
Puis uploadez le fichier.  
Passez à la suite.  
Entrez le nom de la stack. Utilisez votre prénom suivi de "-iam".  
Passez à la suite 2 fois.  
Cochez la case "I acknowledge that AWS CloudFormation might create IAM resources with custom names.".  
Puis cliquez sur "create stack".  
Créez ensuite votre profile pour AWS CLI. 