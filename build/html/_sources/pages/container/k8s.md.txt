# インターネットの「航海士」(k8s)

## kubectlコマンド

### 全てのリソース確認
```shell
kubectl get all
```


### リソース作成
```shell
kubectl create deployment nginx --image nginx:latest
```


### デバック方法
#### ログを確認する
```shell
kubectl logs podの名前
```

#### podのメタデータを確認する
```shell
kubectl describe pod podの名前
```

#### 実際のpodに入って調査する
```shell
kubectl exec -it podの名前 /bin/sh
```

#### エンドポイント確認
```shell
kubectl cluster-info
```

#### ローカルからkubectlにアクセスする
```shell
>>ブラウザからlocalhost:8080でアクセスできるようになる。
kubectl port-forward podの名前 8080:80
```


### k8sの環境について
#### クラスター切り替え
##### 環境を確認する
```shell
kubectx

※ brew install kubectx
```

##### 環境切り替え
```shell
kubectx minikube
```

#### ネームスペース操作
##### ネームスペース確認
````shell
kubens

or 
kubectl get pods --all-namespaces
````

##### ネームスペース作成
```shell
kubectl create namespace ネームスペース名
```

##### ネームスペースの変更
```shell
kubens ネームスペース名
```

##### ネームスペースの区別をする時には

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
    name: nginx
    namespace: <ここにネームスペース名を記述する>
```

##### ネームスペースでpodを確認したい時
```shell
kubectl get pods -n ネームスペース名
```

#### kubectlを設定しているファイルに関して
##### configの有りか
```shell
ls ~/.kube/config
```