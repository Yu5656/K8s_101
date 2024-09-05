# K8sの概観

K8sノード（イメージのインスタンス）は、K8s MasterとK8s Nodeに分かれる。
前者がK8s特有で、コンテナの起動・管理を行う。後者はDockerのコンテナと同じ。

MasterはNodeを次の５つの観点から管理する。これらを**リソース**と呼ぶ。 Nodeの管理は、
Masterに実装された **API（RESTful API）** を介して行う。
[curlコマンド](https://curl.se/)や[xhコマンド](https://github.com/ducaale/xh)で
直接APIを叩くことも可能である。

| リソース                                       | API                   |
| --------------------------------------------- | --------------------- |
| コンテナの実行に関するリソース                     | Workload APIs         |
| 外部ネットワークに接続するサービス（IPマスカレード？） | Service APIs          |
| 設定/機密情報/永続ボリュームのリソース              | Config & Storage APIs |
| セキュリティ/クォータなどのリソース                 | Cluster APIs          |
| 上記以外のリソース                               | Metadata APIs         |


## K8sクラスタの定義

「マニフェスト」に、１つのK8sクラスタの構成として、Master/Node（複製数/ポッド名/イメージ）、
およびリソース情報を記述する。

**詳細は、本の後ほど説明される（ここに追加予定）**

jsonやyamlで書かれている。後者の方がシンプル（括弧ではなく、Pythonのようにインデントで塊を
表現する）。

## K8sクラスタの管理方式の設定

複数のK8sクラスタの情報を記述する。

kubeconfig（`pwd`に存在しない場合は`~/.kube/config`がデフォルト設定）に記述する。
記述する内容は`clusters`、`users`、`contexts`の３つについて。

| 項目             | 詳細                                 |
| --------------- | ------------------------------------ |
| clusters        | 接続先クラスタの情報（WebサーバURL）      |
| users           | ユーザ認証情報                         |
| contexts        | cluster/user/**属するnamespace**を記述 |
| current-context | 自分自身のcontextを書く？？？            |

K8sクラスタのグループに名前（namespace）をつけることができ、それにもとづいて、
クラスタへのユーザのアクセス権限を管理（Role-Based Access Control【RBAC】）したり、
クラスタ間の通信を管理・制御（Network Policy）が可能である。

Namespace/Context/{User, Cluster}という階層構造で、K8sクラスタを識別できる。K8sクラスタの
グループの最小単位がContextであり、ContextをまとめてNamespaceを任意でつけることができる。

Contextの切り替えは`kubectx`、Namespaceは`kubens`というコマンドがある。
いずれも`[kube-krew](https://github.com/kubernetes-sigs/krew/)`からインストール可能。
[こちらのページ](https://github.com/ahmetb/kubectx)を参照。

```sh
# kubectl config user-context docker-desktop と等価
kubectx docker-desktop

# １つ前のContextに戻る
kubectx -

# Context削除
kubectx -d some-context

# Namespaceの切り替え
## kubectl config set-context docker-desktop --namespace=default と等価
kubens default
```
