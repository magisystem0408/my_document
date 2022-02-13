#DNSレコードについて


## Aレコード
基本的なDNSリソースレコード。
あるホストを指し示すドメイン名(FQDN:fully qualified domain name)に対して、対応するIPv4アドレスを定義する。

## TXTレコード
あるホスト名の情報付加を自由に定義するためのもの。
DNSを拡張してプロトコルや仕組みを実装する際に、必要な情報の定義などに使われる

## CNAME
あるドメイン名やホスト名の別名を定義するもの
www.example.jpのホストに対してmobile.example.jp IN CNAME www.example.jpのように指定する

## NSレコード(Name Serverレコード)
DNSで定義されるドメインについての情報の種類の一つで、ドメインのゾーン情報を管理するDNSサーバー
