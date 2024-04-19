# Lustre Deployment Manager scripts for NII-GENIAC

NII-GENIAC (LLM-jp) プロジェクトで使用するLustreクラスタの構築用スクリプトです。
Google CloudのDeployment Managerを使用します。

## 備考

別途HPC Toolkitで構築したslurmクラスタ(VPC名: slurm-sys-net)に相乗りする形でクラスタを構築します。

## 構築方法

```shell
git clone (this repository)
cd (cloned repository)
gcloud deployment-manager deployments create lustre --config lustre.yaml
```
