# Lab 13 : mise en place d'une stack ECS sur FARGATE

## Méthode :

Première étape, il va vous falloir un repository ECR.  
  
```bash

aws --profile <profile>  --region us-east-1 \
  cloudformation deploy --template-file ./ecr.yml \
  --stack-name <stack name>  \
  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
  --parameter-overrides RepositoryName=<repo name>

```
  
Vous remarquerez que deux utilisateurs ont été créé. Vous devez créer un profile en utilisant les credentials générées
pour le "pusher user".  
  
Nous allons maintenant "builder" un conteneur et le pousser sur le repository.

```bash

cd ressources
docker build -t <repository>:php
aws ecr get-login-password --region us-east-1 --profile <pusher profile> | docker login --username AWS --password-stdin <account id>.dkr.ecr.us-east-1.amazonaws.com
docker push <repository>:php

```

Créer un script de déploiement en utilisant un Makefile pour votre stack ecs.  
  
Nous allons ensuite compléter les droits de notre service et les variables d'environnement.  
  
Nous ajouterons une base de données et un cluster redis à cette stack pour les deux derniers labs.  