# SSM Run Command で遊ぶ

## 第28回シェル芸勉強会 大阪サテライトLT
## 2017/4/22
## so

>>>

Powered by
* [reveal.js](https://github.com/hakimel/reveal.js/)
* [GitHub Pages](https://horo17.github.io/aws-nest-call-slide/)

>>>

## `$ whoami`

![so](img/so.png)

* so
* インフラエンジニア
  * 主に AWS 方面
* [@3socha](https://twitter.com/3socha)

---

## [EC2 System Manager](https://aws.amazon.com/jp/ec2/systems-manager/)

![SystemManager](img/system_manager.png)

- 旧称 SSM (Simple System Manager)

>>>

## System Manager 機能

![ssm feature 1](img/ssm_ft1.png)

>>>

## System Manager 機能

![ssm featuret 2](img/ssm_ft2.png)

>>>

## SSM Run Command

* SSM Agent が SSM エンドポイントと通信し、リモートからコマンドを実行
  * Linux : [要 Agent インストール](http://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/ssm-agent.html)
  * Windows : WS2016 / 2016年11月以降に公開された WS2003~WS2012R2 のイメージにはインストール済み
  * セキュリティグループ : OutBound TCP/443 を許可

>>>

## 実行する側の必要権限

* AmazonEC2RoleforSSM ポリシーを参考に
  * s3 PutObject / GetObject が無制限に付いてるのは気にした方が良いかも

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeAssociation",
                "ssm:GetDeployablePatchSnapshotForInstance",
                "ssm:GetDocument",
                "ssm:GetParameters",
                "ssm:ListAssociations",
                "ssm:ListInstanceAssociations",
                "ssm:PutInventory",
                "ssm:UpdateAssociationStatus",
                "ssm:UpdateInstanceAssociationStatus",
                "ssm:UpdateInstanceInformation"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2messages:AcknowledgeMessage",
                "ec2messages:DeleteMessage",
                "ec2messages:FailMessage",
                "ec2messages:GetEndpoint",
                "ec2messages:GetMessages",
                "ec2messages:SendReply"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstanceStatus"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ds:CreateComputer",
                "ds:DescribeDirectories"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:AbortMultipartUpload",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::amazon-ssm-packages-*"
        }
    ]
}
```

>>>

## Linux ホストで `uname -a` を実行

```sh
aws ssm send-command \
  --document-name "AWS-RunShellScript" \
  --instance-ids "i-020ab852e73e487a0" \
  --parameters '
    {
      "commands": [ "uname -a" ],
      "executionTimeout": [ "30" ]
    }' \
  --timeout-seconds 30 \
  --region ap-northeast-1
```

* 実行結果を S3 に吐いてないのであまり意味はない

>>>

## Windows ホストの Powershell で `Get-AWSPowerShellVersion` を実行

```sh
$ aws ssm send-command \
--document-name "AWS-RunPowerShellScript" \
--instance-ids "i-00342466c9d9b3c47" \
--parameters '
{
  "commands": [ "Get-AWSPowerShellVersion" ],
  "executionTimeout": [ "30" ]
}' \
--timeout-seconds 30 \
--region ap-northeast-1
```

