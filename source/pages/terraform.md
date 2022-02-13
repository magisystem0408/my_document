# terraform

## workspace確認
```shell
terraform workspace list
```
## workspaceの新規作成
```shell
terraform workspace new staging
```

## workspaceの切り替え
```shell
terraform workspace select staging
```

## 変数ファイル設定
```shell
terraform plan -var-file="ファイル名.tfvars"
```