# K8sのWorkload APIの区分

以下の区分になっている。これまで、上位下位の区分なく、急にPodという言葉が
出てきたが、Workload API配下の話をしている（著者の説明が不親切）。

1. Pod
2. ReplicationController
3. ReplicaSet
4. Deployment
5. DaemonSet
6. StatefulSet
7. Job
8. CronJob

Podは、複数のDockerコンテナを含むことができる。その際、全てのDockerコンテナは
同一のIPアドレスを共有する。互いをホスト名で区別して通信する（通常のネットワークとは
異なる仕組みで動いている）。ポート番号に名前を付けている感じ？？？

複雑な動作をするコンテナを同一Pod内に配置すると、混乱が起きる。提唱されている
デザイパタンに適合させるのが良いらしい。以下のパタンがある。

- サイドカー・パタン: メインのコンテナを支える補助的なコンテナで構成。例えば、Webサーバのコンテナと、そのログを永続ボリュームに転送するコンテナなど。
- アンバサダ・パタン: 外部システム（ポッドの外）にアクセスする際、中継を集中的に行うコンテナを配置するもの。AWSのQueueサービスのように、疎結合にするQueueが配置されているイメージ。
- アダプタ・パタン: 類似用途のサービス/サーバに対して、変換（アダプト）してくれる中継機を置く構成。例えば[Prometheus](https://prometheus.io/)

Deployment/ReplicaSet/Podという構成が標準的。ローリングアップデート（rolloutと同義）にはDeploymentが必要になる。ReplicaSetは、Pod数を制御する。Podは複数コンテナをまとめる役割を果たす。

１ノード１Podを確保するために使われる。ログモニタリング用のPod走らせる時に使われる。

StatefulSetは、永続ボリュームを使うPodがある時に使われる。

Jobは、指定した並列数で、指定した回数だけ処理が成功するまでPodを起動するのに使われる。CronJobは、Unix系OSのCronと同様、決められた日時、時刻に実行するのに使われる。
