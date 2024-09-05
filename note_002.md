# Dockerの基本

Dockerコンテナ最初の一歩として、以下を順に実行すれば、どのようなことが可能か、
分かってくる。

```sh
docker run hello-world
```

で、いわゆる `Hello World` のDocker版を見ることができる。

```sh
docker run -it ubuntu bash
```

で、`bash`も動作させることが可能！

最初の `hello-world` 実行時に[ユーザガイド](https://docs.docker.com/engine/daemon/)が紹介されていた。

```sh
docker pull centos:7
docker run -it --name test1 centos:7 bash
```

で、名前をつけて実行することができる。

Dockerで実行したプロセスは `docker ps` で一覧表示できる。

```sh
docker ps

CONTAINER ID   IMAGE      COMMAND   CREATED          STATUS          PORTS     NAMES
c90a08b79238   centos:7   "bash"    11 seconds ago   Up 10 seconds             xxx
```

停止させることもできる。

```sh
docker stop c90a08b79238
```

または `NAMES` を指定できる。

```sh
docker stop xxx
```

`docker ps -a` で、過去に走らせたンテナを見ることができる。

```sh
docker ps -a

CONTAINER ID   IMAGE         COMMAND               CREATED             STATUS                         PORTS     NAMES
8686641af0a4   ubuntu        "bash"                4 minutes ago       Exited (137) 3 minutes ago               vigorous_lalande
a3742775907d   centos:7      "bash"                14 minutes ago      Exited (127) 13 minutes ago              brave_edison
b76b3b68151a   centos:7      "zsh"                 15 minutes ago      Created                                  test2
71c81e69a132   centos:7      "bash"                16 minutes ago      Exited (0) 16 minutes ago                determined_roentgen
6b5e6af35273   centos:7      "bash"                16 minutes ago      Exited (0) 16 minutes ago                test1
404e29f89cf9   centos:7      "bash --name test1"   16 minutes ago      Exited (2) 16 minutes ago                festive_chaum
31be01695981   centos:7      "bash --name test1"   17 minutes ago      Exited (2) 17 minutes ago                epic_payne
9617f958a71b   centos:7      "bash"                18 minutes ago      Exited (0) 17 minutes ago                optimistic_roentgen
e1d5659c79a7   ubuntu        "bash"                19 minutes ago      Exited (0) 19 minutes ago                bold_rhodes
c989d9e0a9e5   hello-world   "/hello"              About an hour ago   Exited (0) About an hour ago             priceless_cray
b8ddb7ae7972   ubuntu        "bash"                About an hour ago   Exited (0) About an hour ago             elegant_booth
56f415ab7355   hello-world   "/hello"              About an hour ago   Exited (0) About an hour ago             angry_jones
```

`docker run` が可能なのは、必要なイメージをローカルに取ってきているから。
以下のコマンドで、イメージ名一覧を取得できる。

```sh
docker images

REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
Yu5656/ubuntu   7-git     94d1f13d46e8   8 minutes ago    234MB
ubuntu          7-git     94d1f13d46e8   8 minutes ago    234MB
yu5656/ubuntu   7-git     94d1f13d46e8   8 minutes ago    234MB
centos          7-git     defca47798c9   19 minutes ago   234MB
ubuntu          latest    edbfe74c41f8   3 weeks ago      78.1MB
hello-world     latest    d2c94e258dcb   15 months ago    13.3kB
centos          7         eeb6ee3f44bd   2 years ago      204MB
```

以下のように `REPOSITORY` の名前（`tag`）を付けることができる。

```sh
docker tag ubuntu:7-git yu5656/ubuntu:7-git
```

CONTAINER IDを指定してイメージを更新できる。そのイメージをDocker Hubにpushできる！
https://hub.docker.com のユーザ名を最初に付している。ユーザ名は小文字であることに注意。

```sh
docker login
docker commit 8686641af0a4 yu5656/ubuntu:7-git
docker push yu5656/ubuntu:7-git
```

CONTAINER IDを指定して、再度、コンテナを走らせることができる。

```sh
docker start <container-id>
```

不要になった、コンテナは「CONTAINER ID」または「NAMES」を指定して削除できる。

```sh
docker rm <container_id>
```

同様に、イメージは「IMAGE ID」または「REPOSITORY:TAG」を指定して削除できる。

```docker
docker rmi REPOSITORY:TAG
```

コンテナのIPAddressも取得できる。

```sh
docker inspect --format='{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ID
```

CONTAINER IDかNAMESを指定する。

```sh
docker exec -it <Name/ContainerID> bash
```

で `docker run -it ...` で立ち上げたLinuxコンテナのプロセスに合流する。


## コマンドの覚え書き

**docker pull [ImageID:Tag]**

レジストリのイメージをローカルにダウンロードする。
レジストリは、レポジトリを複数もつ、保存サービスのこと。

パブリックなレジストリは以下が知られている。

- [Docker Hub](https://hub.docker.com/)
- [Quay](https://quay.io/)

有料のレジストリとして以下が知られている。

- [Amazon ECR](https://aws.amazon.com/jp/ecr/)
- IBM ...
- Google ...

Dockerのコマンド群だけでは動作しない。Docker HubからDockerアプリを導入して、
デーモンとして動作させる必要がある。


**docker build**

ベースのイメージから、機能を加えた新イメージを作る

OOPLの「クラス」に相当するもの

## docker run [ImageID:Tag]

作成したイメージから、コンテナを作成（実行）する
OOPLの「インスタンス」に相当するもの。OS上では「プロセス」として存在する。


**docker exec [ImageID:Tag]**

既に `docker run` されているコンテナと合流する


**docker stop/kill [ContainerID/Name]**

`stop` は正常に終了させる。`kill` は強制停止させる


**docker start [ImageID:Tag]**


**docker commit [ContainerID/Name] [ImageID:Tag]**

コンテナ状態をイメージに書き込む


**docker push [ImageID:Tag]**

`docker login` しているレジストリに、イメージをプッシュする


**docker rm [ContainerID/Name]**

コンテナを削除する

**docker rmi [ImageID:Tag]**

イメージを削除する


## Dockerfileの書き方

**追加予定**

