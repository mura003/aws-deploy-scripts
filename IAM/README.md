# IAM設定

## IAM User

## 定数定義
```
export IAM_USER_NAME=IAMユーザー名
```

## ポリシー設定
```
aws iam create-user --user-name ${IAM_USER_NAME}
aws iam attach-user-policy --user-name ${IAM_USER_NAME}--policy-arn arn:aws:iam::aws:policy/AmazonSQSFullAccess
aws iam attach-user-policy --user-name ${IAM_USER_NAME} --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-user-policy --user-name ${IAM_USER_NAME} --policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
aws iam attach-user-policy --user-name ${IAM_USER_NAME} --policy-arn arn:aws:iam::aws:policy/AmazonSNSFullAccess

# 確認
aws iam list-attached-user-policies --user-name ${IAM_USER_NAME}
``` 

### AWS アクセスキーとシークレットを作成
```
aws iam create-access-key --user-name ${IAM_USER_NAME} --query 'AccessKey.[AccessKeyId,SecretAccessKey]'
```