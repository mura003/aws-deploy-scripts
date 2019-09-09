# Kinesis設定

## 定数定義
```
export STREAM_NAME=ストリーム名
export SHARD_COUNT=シャード数
```

## 確認
```
aws kinesis describe-stream --stream-name ${STREAM_NAME}
aws kinesis list-streams|grep ${STREAM_NAME}
```

## 作成
```
aws kinesis create-stream --stream-name ${STREAM_NAME} --shard-count ${SHARD_COUNT}
```