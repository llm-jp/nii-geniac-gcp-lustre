# Lustre Deployment Manager scripts for NII-GENIAC

NII-GENIAC (LLM-jp) プロジェクトで使用するLustreクラスタの構築用スクリプトです。
Google CloudのDeployment Managerを使用します。

別途HPC Toolkitで構築した、slurm管理されたGPUクラスタの存在するVPCに相乗りする形でLustreクラスタを構築します。
こちらで作成されたLustreクラスタはslurm管理下には置かれません。
また、作成したストレージがGPUノード上に自働でマウントされることはないため、別途マニュアルでマウント処理を行う必要があります。

## 構築方法

```shell
git clone (this repository)
cd (cloned repository)
gcloud deployment-manager deployments create lustre --config lustre.yaml
```

適切な権限を持つアカウントで上記コマンドを実行することで（特に `gcloud`）、
数分程度でLustreクラスタが構築されます。

コマンド中の `lustre` はGoogle Cloudが管理するデプロイ単位の名前です。
複数のLustreクラスタを構築する場合はそれぞれ別の名前を設定する必要があります。

`lustre.yaml`に実際に構築するクラスタの構成情報があります。最低限、下記項目を編集します。

* `resources.name`: 作成するリソースグループの名前。必要なければデプロイメント名と同じにする。
* `resources.properties.cluster_name`: クラスタ名。必要なければデプロイメント名と同じにする。
* `zone`: リソースを作成するサーバセンター。GPUクラスタと同じにする。
* `vpc_net`: リソースを作成する既存VPC名。GPUクラスタと同じにする。
* `vpc_subnet`: リソースを作成する既存VPCサブネット名。GPUクラスタと同じにする。
* `oss_node_count`: OSS（ファイル保管先）の台数。
* `ost_per_oss`：OSSに接続されるディスクの個数。
* `ost_disk_size_gb`：OSSに接続されるディスクあたりの容量。

構築されるストレージの全容量は `oss_node_count * ost_per_oss * ost_disk_size_gb` GBになります。
その他の設定項目は必要に応じて編集します。
不明点は @odashi に問い合わせてください。

## 削除方法

HPC Toolkitにより構築したVPCを削除する**前に**、Lustreクラスタを完全に削除する必要があります。

```shell
# デプロイメント `lustre` を削除
gcloud deployment-manager deployments delete lustre
```

`lustre` は構築したデプロイ単位の名前です。適宜変更して下さい。
