# K8sシステムの構築ツール

DockerコンテナはDockerfileで定義を書けば十分だった。

K8sの複数ポッドからなるシステム構築は、構成が複雑になるためか、いくつかツールが用意されている。
これらも、結局、ツールに応じた、構成の設定ファイルを書くことになるのだが。

【１】ローカルK8sとして、[Docker Desktop for Mac/Windows](https://docs.docker.com/desktop/install/mac-install/)と
[kind](https://kind.sigs.k8s.io/)を使う。前者は、Dockerを使うための、デーモンとして動作する。

後者は[brew](https://brew.sh/)から導入できる。

```sh
brew install kind
```


【２】パブリッククラウド上のマネジドK8sとして、
[Google K8s Engine（GKE）](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview)
を選択する。ツールの導入は以下のコマンドでできる。

```sh
curl https://sdk.cloud.google.com/ | bash
```

この後、環境変数を適切に設定してパスを通さないとコマンド群が呼べない。

```sh
gcloud init # 認証ページに飛ばされる
gcloud container get-server-config --zone asia-northeast1-a
# この後に出てくるエラーのURLでK8s Engine APIを有効にする
# クレジットカード番号が必要になる
```

この後、以下のコマンドは正常に出力する。

```sh
gcloud config get-value core/account
gcloud config get-value core/project
gcloud container get-server-config --zone asia-northeast1-a
```

参照: [Google Cloud認定資格](https://www.amazon.co.jp/dp/4295017639)


【３】K8s構築ツールとして、Kubeadm/FlannelやRancherなどがある。構築ツールで、システムを構築して、好きな環境で
走らせるためのツールになる。これが、いちばん汎用的と思うが、ローカルK8sは学習用、パブリッククラウド上は本番環境という
位置づけっぽい。どうやら他人が適当が構築したK8sシステムを、自分のクラウド上では走らせたくないらしい。

## コマンド補完設定

docker/kubectl/gloudは、端末上のコマンドとして提供されている。サブコマンドやオプションが多岐に渡るため、
補完設定ができるようになっている。

以下で作られるファイルを.zshrcなどから読み込ませれば良い。

```sh
docker completions zsh >> $HOME/.zshrc.docker.completions
kubectl completions zsh >> $HOME/.zshrc.kubectl.completions
```

gcloudに関しては、インストーラが勝手にやってくれる。


## その他のコマンドラインツール

本に倣い、[kubeadm/kubelet](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)を
導入したかったが、Linux（Debian系/Red Hat系）でないとダメらしい。

brew search xxxで調べると、以下のツール群が見つかる。

- [kubecm](https://kubecm.cloud/en-us/introduction)
- [kubent](https://github.com/doitintl/kube-no-trouble)
- [kubekey](https://github.com/kubesphere/kubekey)

学習用に[K8s playground](https://labs.play-with-k8s.com/)サービスが提供されている。
