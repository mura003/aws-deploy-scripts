# Route53設定

## 定数定義
```
export HOSTEDZONE_ID=ゾーンID
export DNS_NAME=DNS名称
export CNAME_VALUE=レコード(今回はCNAME)
export ELB_NAME=LBに設定想定
```

## 確認
```
# ホストゾーン設定が返ってくる
aws route53 get-hosted-zone --id ${HOSTEDZONE_ID}|jq -cr '.HostedZone|[.Id, .Name]'
# 設定がまだなので返ってこない
dig ${DNS_NAME}
# grepで引っかかる
aws elb describe-load-balancers --load-balancer-names ${ELB_NAME}|jq -cr '.LoadBalancerDescriptions[]|[.LoadBalancerName, .DNSName]'|grep ${CNAME_VALUE}

```

## Route53設定

```
cat <<EOT > /tmp/create_dns_recordset.json
{
  "Changes": [
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "${DNS_NAME}",
        "Type": "CNAME",
        "TTL": 60,
        "ResourceRecords": [
          {
            "Value": "${CNAME_VALUE}"
          }
        ]
      }
    }
  ]
}
EOT

less /tmp/create_dns_recordset.json

aws route53 list-hosted-zones|jq -cr '.HostedZones[]|[.Id, .Name]'|grep ${DNS_NAME}

aws route53 change-resource-record-sets --hosted-zone-id ${HOSTEDZONE_ID} --change-batch file:///tmp/create_dns_recordset.json

# 確認
aws route53 list-hosted-zones|jq -cr '.HostedZones[]|[.Id, .Name]'|grep ${DNS_NAME}
```