---
title: ポートとプロトコル
content_type: reference
weight: 40
---

パブリッククラウドにおける仮想ネットワークや、物理ネットワークファイアウォールを持つオンプレミスのデータセンターのような、ネットワークの境界が厳格な環境でKubernetesを実行する場合、Kubernetesのコンポーネントが使用するポートやプロトコルを認識しておくと便利です。

## コントロールプレーン {#control-plane}


| プロトコル | 通信の向き | ポート範囲  | 目的                    | 使用者                    |
|------------|------------|-------------|-------------------------|---------------------------|
| TCP        | Inbound    | 6443        | Kubernetes API server   | 全て                       |
| TCP        | Inbound    | 2379-2380   | etcd server client API  | kube-apiserver, etcd      |
| TCP        | Inbound    | 10250       | Kubelet API             | 自身, コントロールプレーン       |
| TCP        | Inbound    | 10259       | kube-scheduler          | 自身                      |
| TCP        | Inbound    | 10257       | kube-controller-manager | 自身                      |

etcdポートはコントロールプレーンノードに含まれていますが、独自のetcdクラスターを外部またはカスタムポートでホストすることもできます。

## ワーカーノード {#node}

| プロトコル | 通信の向き | ポート範囲  | 目的                  | 使用者                  |
|------------|------------|-------------|-----------------------|-------------------------|
| TCP        | Inbound    | 10250       | Kubelet API           | 自身, コントロールプレーン     |
| TCP        | Inbound    | 30000-32767 | NodePort Services†    | 全て                     |

† [NodePort Services](/ja/docs/concepts/services-networking/service/)のデフォルトのポート範囲。


すべてのデフォルトのポート番号が書き換え可能です。
カスタムポートを使用する場合、ここに記載されているデフォルトではなく、それらのポートを開く必要があります。

よくある例としては、API Serverのポートを443に変更することがあります。
または、デフォルトポートをそのままにし、API Serverを443でリッスンしているロードバランサーの後ろに置き、APIサーバのデフォルトポートにリクエストをルーティングする方法もあります。
