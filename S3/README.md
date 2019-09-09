# S3設定

## 定数定義
```
export BUCKET_NAME=バケット名
```

## 確認
```
aws s3api head-bucket --bucket ${BUCKET_NAME}
# ない場合に404が帰ってくる
```

## バケット作成

```
aws s3api create-bucket --bucket ${BUCKET_NAME} --region ap-northeast-1
aws s3api head-bucket --bucket ${BUCKET_NAME}
```