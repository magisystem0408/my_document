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