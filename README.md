# SSM Run Command で遊ぶ

## 第28回シェル芸勉強会 大阪サテライトLT
## 2017/4/22
## so

>>>

Powered by
* [reveal.js](https://github.com/hakimel/reveal.js/)
* [GitHub Pages](https://horo17.github.io/ssm-run-command-slide/)

>>>

## `$ whoami`

![icon](img/so.png)

* so
* インフラエンジニア (AWS)
* Twitter: @3socha

---

## SSM Run Command

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

