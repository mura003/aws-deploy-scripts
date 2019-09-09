# SQS設定

## 定数定義
```
export ACCOUNT_ID=アカウントID
export QUEUE_NAME=キュー名
export DEAD_LETTER_QUEUE_NAME=デッドレターキュー
```

## 確認
```
aws sqs list-queues --queue-name-prefix ${QUEUE_NAME}
```

## 作成
```
aws sqs create-queue --queue-name ${QUEUE_NAME}
```

## ポリシー変更

### ポリシー 例
```
cat <<EOT > ./sqs-policy.json
{
  "DelaySeconds": "10",
  "MaximumMessageSize": "131072",
  "MessageRetentionPeriod": "259200",
  "ReceiveMessageWaitTimeSeconds": "20",
  "RedrivePolicy": "{\"deadLetterTargetArn\":\"arn:aws:sqs:ap-northeast-1:${ACCOUNT_ID}:${DEAD_LETTER_QUEUE_NAME}\",\"maxReceiveCount\":\"1000\"}",
  "VisibilityTimeout": "60"
}
EOT
```

```
aws sqs set-queue-attributes --queue-url https://sqs.ap-northeast-1.amazonaws.com/${ACCOUNT_ID}/${QUEUE_NAME} --attributes file://sqs-policy.json
```