# gitops-manifest
## このリポジトリについて
このリポジトリはSoftware Design 2021年9月号 短期連載「GitOpsで作るKubernetesのCI/CD環境」のマニフェストリポジトリのサンプルです。

* アプリリポジトリのサンプル
    * https://github.com/amaya382/gitops-sampleapp
* 初回デプロイリクエスト
    * https://github.com/amaya382/gitops-manifest/pull/1
* Serviceの追加
    * https://github.com/amaya382/gitops-manifest/pull/2


## 事前準備
サンプルを動かすためのKubernetes環境は [kind](https://kind.sigs.k8s.io) (`v0.11.1`) を利用して作成できます。Dockerとkubectlが利用できる環境で以下のコマンドを実行してください。


### kindのダウンロード
```console
$ curl -sSL https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64 -o kind # Linux, WSL2の場合
$ # curl -sSL https://kind.sigs.k8s.io/dl/v0.11.1/kind-darwin-amd64 -o kind # macOSの場合
$ chmod +x ./kind
```

### Kubernetes環境の作成
```console
$ ./kind create cluster --name dev # 開発環境
Creating cluster "dev" ...
 ✓ Ensuring node image (kindest/node:v1.21.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-dev"
You can now use your cluster with:

kubectl cluster-info --context kind-dev 

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂

$ ./kind create cluster --name prd # 本番環境
(省略)
```

### Kubernetes環境の確認
kindで作成されたKubernetes環境には `kind-{名前}` コンテキストでアクセスできます。

```console
$ kubectl cluster-info --context kind-dev
Kubernetes control plane is running at https://127.0.0.1:45811
CoreDNS is running at https://127.0.0.1:45811/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

$ kubectl cluster-info --context kind-prd
(省略)
```


## クリーンアップ
本サンプルは以下のコマンドで削除できます。

### デプロイしたアプリだけを削除
以下のコマンドではArgo CDを利用してデプロイしたアプリだけを削除します。クローンした本リポジトリのルートディレクトリで実行する必要があります。
```console
$ kubectl --context kind-dev delete -f cd/dev/sampleapp.yaml # 開発環境
$ kubectl --context kind-prd delete -f cd/prd/sampleapp.yaml # 本番環境
```

### Kubernetes環境ごと削除
以下のコマンドではkindで作成したKubernetes環境ごと削除を行います。
```console
$ ./kind delete cluster --name dev # 開発環境の削除
$ ./kind delete cluster --name prd # 本番環境の削除

$ rm ./kind # kindコマンド自体の削除
```



## リンク
* [Software Design 2021年9月号](https://gihyo.jp/magazine/SD/archive/2021/202109)
