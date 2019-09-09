# Kinesis設定

## 定数定義
```
export ACCOUNT_ID=アカウントID
export TOPIC_NAME=トピック名
export NOTIFICATION_EMAIL=通知メール
export LAMBDA_FUNCTION_NAME=Lamnda名
export SQS_QUEUE_NAME=キュー名
```

## 確認
```
aws sns list-topics |grep ${TOPIC_NAME}
```

## 作成
```
aws sns create-topic --name ${TOPIC_NAME}
```

## subscribe登録

```
# email
aws sns subscribe --topic-arn arn:aws:sns:ap-northeast-1:${ACCOUNT_ID}:${TOPIC_NAME} --protocol email --notification-endpoint ${NOTIFICATION_EMAIL}

# lambda
aws sns subscribe --topic-arn arn:aws:sns:ap-northeast-1:${ACCOUNT_ID}:${TOPIC_NAME} --protocol lambda --notification-endpoint arn:aws:lambda:ap-northeast-1:${ACCOUNT_ID}:function:${LAMBDA_FUNCTION_NAME}

# sqs
aws sns subscribe --topic-arn arn:aws:sns:ap-northeast-1:${ACCOUNT_ID}:${TOPIC_NAME} --protocol sqs --notification-endpoint arn:aws:sqs:ap-northeast-1:${ACCOUNT_ID}:${SQS_QUEUE_NAME}
```

```
aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:ap-northeast-1:${ACCOUNT_ID}:${TOPIC_NAME}
```