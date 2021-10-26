# LAB 4: Première lambda Hello World

## Méthode:

Avant tout, chaque lamdba assume une role IAM.  
Ajoutez donc au template, le role :  
  
```

  HelloLambdaRole:
    Type: "AWS::IAM::Role"
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - lambda.amazonaws.com
            Action:
              - sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

```

Ajoutons maintenant notre fonction :  
  
```

  HelloLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: !Sub "HelloLambdaFunction${AWS::StackName}"
      Role: !GetAtt HelloLambdaRole.Arn
      Runtime: python3.7
      Handler: index.my_handler
      Code:
        ZipFile: |
          def my_handler(event, context):
            message = 'Hello World!'
            return message

```

Nous allons maintenant déployer cette étape :  
  
```bash

aws --profile <my profile> \   
    --region us-east-1 cloudformation deploy \          
    --template-file ./lambda.yml \        
    --stack-name <TO REPLACE> \          
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --role-arn <to replace>

```
  
Nous allons exécuter notre lambda:  
  
```bash

aws --profile <to replace> \
--region us-east-1 lambda invoke \
--invocation-type RequestResponse \
--function-name <to replace> \
--log-type Tail outputfile.txt

more outputfile.txt

```
  
Nous allons maintenant ajouter le déclencheur.
  
```

  LambdaSchedule:
    Type: AWS::Events::Rule
    Properties:
      Description: "A schedule for the Lambda function.."
      ScheduleExpression: "rate(5 minutes)"
      State: ENABLED
      Targets:
        - Arn: !GetAtt HelloLambdaFunction.Arn
          Id: LambdaSchedule
          
  LambdaSchedulePermission:
    Type: AWS::Lambda::Permission
    Properties:
      Action: 'lambda:InvokeFunction'
      FunctionName: !GetAtt HelloLambdaFunction.Arn
      Principal: 'events.amazonaws.com'
      SourceArn: !GetAtt LambdaSchedule.Arn
          
```
