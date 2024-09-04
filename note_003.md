# K8sの概観

K8sノード（イメージのインスタンス）は、K8s MasterとK8s Nodeに分かれる。
前者がK8s特有で、コンテナの起動・管理を行う。後者はDockerのコンテナと同じ。

MasterはNodeを次の５つの観点から管理する。これらを**リソース**と呼ぶ。 Nodeの管理は、
Masterに実装された***API（RESTful API）**を介して行う。curlコマンドなどで、
直接APIを叩くことも可能である。

| リソース                                       | API                   |
| --------------------------------------------- | --------------------- |
| コンテナの実行に関するリソース                     | Workload APIs         |
| 外部ネットワークに接続するサービス（IPマスカレード？） | Service APIs          |
| 設定/機密情報/永続ボリュームのリソース              | Config & Storage APIs |
| セキュリティ/クォータなどのリソース                 | Cluster APIs          |
| 上記以外のリソース                               | Metadata APIs         |

## クラスタの識別子Namespace

K8sクラスタに名前をつけることができ、それにもとづいて、クラスタへのユーザのアクセス権限を
管理（Role-Based Access Control【RBAC】）したり、クラスタ間の通信を管理・制御
（Network Policy）が可能である。
