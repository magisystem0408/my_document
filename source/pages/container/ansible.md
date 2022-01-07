# Ansible

## ansibleとは
ansibleサーバーから別のサーバーの設定を自動化してくれる。


### playbook(yml)による基本的な実行
```yaml
# 接続先
host: 10.91.77.16
#osの条件分岐などに使う
gather_facts: yes
#どのuserでログインするか
remote_user: root
vars:
  ansible_password:password

# 作業を書く
tasks:
  - name: Make somefolder
    file:
      path: "/root/test"
#こうなっている状態を書く
      state: directory

```
### playbookの実行
```shell
ansible-playbook yamlの名前
```


### playbookのモジュール使い方
1. apacheのインストール
2. apacheのconf配置
3. トップページの配置
4. apacheのサービス有効化


```yaml
hosts: 10.91.77.16
gather_facts: yes
remote_user: root
vars:
  ansible_password: password
tasks:
  - name: Apache2のインストール

#aptでインストールできる
    apt:
#Apache2がインストールされていてかつ最新版にする
      name: Apache2
      state: latest
  - name: confファイルを配置
    copy:
#コピーするものを書く      
      src: apache2.conf
      #配置先
      dest: /etc/apache2/apache2.conf
  - name: トップページの配置
    copy:
      src: index.html
      dest: /var/www/index.html
  - name: apacheのサービスの有効化
    service:
      name: apache2
      state: stated
      enabled: yes
```
>コピー元はansibleディレクトリ配下のfilesを参照するようになっているのでその中に入れる。


### 設定の再読み込みなどをする方法

1. serviceモジュールのstateをrestartedにする。
   - 設定変更が行われていなくても常にchangedになってしまう。
2. commandモジュールで直接サービス再起動のコマンドを入れる
   -  1と同様
3. handlerを使用する


```yaml
hosts: 10.91.77.16
gather_facts: yes
remote_user: root
vars:
  ansible_password: password
tasks:
  - name: Apache2のインストール
    apt:
      name: Apache2
      state: latest
  - name: confファイルを配置
    copy:
      src: apache2.conf
      dest: /etc/apache2/apache2.conf
#    ここの値がchangedになった時、handlerが呼ばれる。
    notify: restart apache2
  - name: トップページの配置
    copy:
      src: index.html
      dest: /var/www/index.html
      
  - name: apacheのサービスの有効化
    service:
      name: apache2
      state: stated
      enabled: yes
      
handlers:
  - name: restart apache2
    service:
        name: apache2
        state: restated
```

### 相手先のサーバーに認証を通す方法
1. playbookにパスワードをベタがき
    - セキュリティ面からよくない
2. Vaultで暗号化したパスワードをPlaybookに書く
```shell
暗号化
ansible-vault encrypt_string "暗号化したい文字列" --name "変数名"

実行
ansible-playbook yamlファイル --ask-vault-pass
```
apache2
3. 実行時に対話コマンドでパスワード入力
4. 鍵認証で事前に認証を通しておく<CICDパイプラインを組む時、推奨>


#### sshのフィンガープリント
相手先のサーバーを覚えておいて次回そのサーバーに接続をする時、確実に対象が同じサーバーであることを
確認するためのもの



### Roleでplaybookに分ける
#### フォルダ構成
```
※各フォルダのmain.yamlがデフォルトで読み込まれる。
※全てのディレクトリがあるといったことではない。

- defalut　変数のデフォルトの値
- files　配置するファイルなど
- handlers 最後実行したいhander処理
- meta Roleの依存関係
- tasks 実行するタスク
- templates templateモジュールで使うファイル
- test　テストコードを置く
- vars 状況に応じて読み込む変数を書く
```