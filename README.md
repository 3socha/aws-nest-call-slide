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
* インフラエンジニア (AWS)
* [@3socha](https://twitter.com/3socha)
* 

---

## EC2 System Manager

![SystemManager](img/system_manager.png)

- 旧称 SSM (Simple System Manager)

>>>

## SystemManager 機能

![ssm_ft1](img/ssm_ft1.png)

>>>

## SystemManager 機能

![ssm_ft2](img/ssm_ft2.png)

>>>

- ssm-agent (Linux) と通信し、リモートからコマンドを実行
- 

>>>


```sh
$ aws ssm send-command \
--document-name "AWS-RunPowerShellScript" \
--instance-ids "i-00342466c9d9b3c47" \
--parameters '
{
  "commands": [ "Get-AWSPowerShellVersion" ],
  "executionTimeout": [ "10" ]
}' \
--timeout-seconds 30 \
--region ap-northeast-1
```

