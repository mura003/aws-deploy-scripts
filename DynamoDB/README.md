# DynamoDB作成

## 定数定義
```
export TABLE_NAME=テーブル名
export HASH_KEY_NAME=ハッシュキー名
export RANGE_KEY_NAME=レンジキー名
```

## DynamoDB確認
```
aws dynamodb list-tables --region ap-northeast-1
```

## テーブル定義(オンデマンド化するのでキャパシティは1)
```
cat <<EOT > ./table-config.json
{
  "TableName": "${TABLE_NAME}",
  "AttributeDefinitions": [
    {
      "AttributeName": "schema",
      "AttributeType": "S"
    },
    {
      "AttributeName": "templateName",
      "AttributeType": "S"
    }
  ],
  "KeySchema": [
    {
      "AttributeName": "${HASH_KEY_NAME}",
      "KeyType": "HASH"
    },
    {
      "AttributeName": "${RANGE_KEY_NAME}",
      "KeyType": "RANGE"
    }
  ],
  "ProvisionedThroughput": {
    "WriteCapacityUnits": 1,
    "ReadCapacityUnits": 1
  }
}
EOT

```

## 作成
```
# 作成
aws dynamodb create-table --cli-input-json file://table-config.json --region ap-northeast-1

# 確認
aws dynamodb describe-table --table-name ${TABLE_NAME} --region ap-northeast-1

# オンデマンド化
aws dynamodb update-table --table-name ${TABLE_NAME} --billing-mode PAY_PER_REQUEST --region ap-northeast-1
```