# Helm Chart for GitLab

## Introduction

[GitLab社区版](https://about.gitlab.com/)是一个用于编码, 测试和部署的应用程序. 它为Git存储库管理提供了细粒度的访问控制, 代码审查, 问题跟踪, 活动源, 维基和持续集成.

此 chart 定义了 GitLab Community Edition 组件. 这包括:

- A [GitLab Omnibus](https://docs.gitlab.com/omnibus/) Pod
- Redis
- Postgresql

## Prerequisites

- Kubernetes cluster 1.10+ with Beta APIs enabled
- Kubernetes Ingress Controller is enabled
- Helm 2.8.0+

## Installing the Chart

要安装版本名为 "my-release" 的 chart, 请运行:

```
$ helm install --name my-release \
   --set externalURL=http://your-domain.com stable/gitlab-ce
```

{{% notice info %}}
执行 `helm search gitlab` 来搜索 `gitlab-ce` chart 的名称
{{% /notice %}}

> **Tip**: 使用 `helm list` 列出所有版本

## Uninstalling the Chart

卸载/删除 `my-release`:

```
$ helm delete my-release
```

该命令将删除与 `chart` 关联的所有 `Kubernetes` 资源并删除该版本. 

## 配置

可以到 [values.yaml](values.yaml) 中查看所有配置的默认值, 使用 `helm install` 的 `--set key = value [, key = value]` 参数设置每个参数. 例如,

```
$ helm install --name my-release \
    --set externalURL=http://your-domain.com,gitlabRootPassword=pass1234 \
    stable/gitlab-ce
```

或者, 可以在安装 `chart` 时提供指定参数值的 `YAML` 文件. 例如:

```
$ helm install --name my-release -f values.yaml .
```

> **Tip**: 你可以使用默认配置文件 [values.yaml](values.yaml)

### Registry address

In a private network environment, access to the Internet may be limited and a local registry might be used. To specify the registry address. For a registry that can be accessed using the `10.0.0.2`:

```
--set global.registry.address=10.0.0.2
```

## 数据存储

在安装 chart 前，请先确认将用哪种方式来存储数据:

1. Persistent Volume Claim (建议)
2. Host path

### Persistent Volume Claim

如果 k8s 集群已经有可用的 StorageClass 和 provisioner，在安装 chart 过程中会自动创建 pvc 来存储数据。
想了解更多关于 StorageClass 和 PVC 的内容，可以参考 [Kubernetes Documentation](https://kubernetes.io/docs/concepts/storage/storage-classes/)

**在部署过程中创建 pvc**

```
--set portal.persistence.enabled=true \
--set portal.persistence.storageClass=default \
--set database.persistence.enabled=true \
--set database.persistence.storageClass=default \
--set redis.persistence.enabled=true \
--set redis.persistence.storageClass=default \
```

*storageClass 可以不用传，默认为 default*

**使用一个已存在的 pvc**

```
--set portal.persistence.enabled=true \
--set portal.persistence.existingClaim=<pvc name> \
--set database.persistence.enabled=true \
--set database.persistence.existingClaim=<pvc name> \
--set redis.persistence.enabled=true \
--set redis.persistence.existingClaim=<pvc name> \
```

### Host path

如果集群中没有 provision, 可以用如下方式替代:

**存储数据到指定的 node 中**

```
--set portal.persistence.enabled=false \
--set portal.persistence.host.nodeName=<node name> \
--set portal.persistence.host.path=<path on host to store data>
--set database.persistence.enabled=false \
--set database.persistence.host.nodeName=<node name> \
--set database.persistence.host.path=<path on host to store data>
--set redis.persistence.enabled=false \
--set redis.persistence.host.nodeName=<node name> \
--set redis.persistence.host.path=<path on host to store data>
```

如果禁用了 `persistence`, 并且没有配置 `host`, 则卷的内容只会持续与 `Pod` 一样长. 升级或更改某些设置可能会导致数据丢失, 不建议这样操作, 但如果只是测试，不在乎数据，可如下配置：

```
--set portal.persistence.enabled=false \
--set database.persistence.enabled=false \
--set redis.persistence.enabled=false \
```

### Domain

部署 gitlab 时候，需要指定 gitlab 的访问地址, 您必须传入 `externalURL`, 否则您最终将无法正常运行。

如果没有可用的域名，也可以通过 `<nodeIP>:<nodePort>` 的方式来访问，不过需要在部署时候，提供 `nodePort` 参数:

```
--set externalURL=http://<nodeIP>:<nodePort> \
--set service.type=NodePort \
--set service.ports.http.nodePort=<nodePort>
```

nodePort 的值应该在 `30000` 到 `32767` 中间取值，不要与集群其它服务端口冲突。


## 其它配置

完整的 Helm chart 配置，执行 `helm fetch --untar -d <目标目录> stable/gitlab-ce` 来下载 Gitlab CE Helm chart。进去目录就可以看到 chart中的 `values.yaml` 文件
{{% notice info %}}
注意：目标目录需要提前创建好
{{% /notice %}} 