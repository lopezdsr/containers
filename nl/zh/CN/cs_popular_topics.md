---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}




# {{site.data.keyword.containerlong_notm}} 的热门主题
{: #cs_popular_topics}

与 {{site.data.keyword.containerlong}} 中的内容保持同步。了解要探索的新功能、试用技巧或其他开发者目前在产品中发现有用的一些热门主题。
{:shortdesc}

## 2018 年 10 月的热门主题
{: #oct18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 8 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>10 月 25 日</td>
<td>[在米兰提供了专区](cs_regions.html)</td>
<td>欢迎使用适用于付费集群的欧洲中部区域意大利米兰的新专区。先前，米兰仅可用于免费集群。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>10 月 22 日</td>
<td>[新的伦敦多专区位置 `lon05`](cs_regions.html#zones)</td>
<td>伦敦多专区大城市的 `lon02` 专区替换为新的 `lon05` 专区，新专区的基础架构资源比 `lon02` 更丰富。请使用 `lon05` 专区创建新的多专区集群。支持使用 `lon02` 的现有集群，但新的多专区集群必须改为使用 `lon05`。</td>
</tr>
<tr>
<td>10 月 5 日</td>
<td>与 {{site.data.keyword.keymanagementservicefull}} 集成</td>
<td>您可以通过[启用 {{site.data.keyword.keymanagementserviceshort}} (beta)](cs_encrypt.html#keyprotect) 对集群中的 Kubernetes 私钥进行加密。</td>
</tr>
<tr>
<td>10 月 4 日</td>
<td>现在，[{{site.data.keyword.registrylong}} 已与 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry/iam.html#iam) 集成</td>
<td>您可以使用 IAM 来控制对注册表资源的访问，例如拉取、推送和构建映像。创建集群时，还会创建注册表令牌，以便集群可以使用注册表。因此，您需要注册表的**管理员**平台管理角色才能创建集群。要对注册表帐户启用 IAM，请参阅[对现有用户启用策略强制实施](/docs/services/Registry/registry_users.html#existing_users)。</td>
</tr>
<tr>
<td>10 月 1 日</td>
<td>[资源组](cs_users.html#resource_groups)</td>
<td>您可以使用资源组将 {{site.data.keyword.Bluemix_notm}} 资源分成管道、部门或其他分组，以帮助分配访问权和计量表用途。现在，{{site.data.keyword.containerlong_notm}} 支持在 `default` 组或您创建的其他任何资源组中创建集群！</td>
</tr>
</tbody></table>

## 2018 年 9 月的热门主题
{: #sept18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 8 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>9 月 25 日</td>
<td>[提供了新专区](cs_regions.html)</td>
<td>现在，甚至有更多应用程序部署位置选项可供您选择！
<ul><li>欢迎使用美国南部区域圣何塞的两个新专区：`sjc03` 和 `sjc04`。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于此专区以及您集群所在区域内的其他专区。</li>
<li>现在，通过两个新的 `tok04` 和 `tok05` 专区，可以在亚太地区北部区域的东京[创建多专区集群](cs_clusters_planning.html#multizone)。</li></ul></td>
</tr>
<tr>
<td>9 月 5 日</td>
<td>[在奥斯陆提供了专区](cs_regions.html)</td>
<td>欢迎使用欧洲中部区域挪威奥斯陆的新专区。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
</tbody></table>

## 2018 年 8 月的热门主题
{: #aug18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 8 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>8 月 31 日</td>
<td>{{site.data.keyword.cos_full_notm}} 现在与 {{site.data.keyword.containerlong}} 相集成</td>
<td>使用 Kubernetes 本机持久性卷声明 (PVC) 以向集群供应 {{site.data.keyword.cos_full_notm}}。{{site.data.keyword.cos_full_notm}} 最适合用于读取密集型工作负载，以及在多专区集群中跨多个专区存储数据。从在集群上[创建 {{site.data.keyword.cos_full_notm}} 服务实例](cs_storage_cos.html#create_cos_service)和[安装 {{site.data.keyword.cos_full_notm}} 插件](cs_storage_cos.html#install_cos)开始。</br></br>不确定哪种存储解决方案适合您？请从[此处](cs_storage_planning.html#choose_storage_solution)开始分析数据并针对数据选择相应的存储解决方案。</td>
</tr>
<tr>
<td>8 月 14 日</td>
<td>将集群更新为 Kubernetes V1.11 以分配 pod 优先级</td>
<td>在将集群更新为 [Kubernetes V1.11](cs_versions.html#cs_v111) 后，可利用新功能，例如，使用 `containerd` 提高的容器运行时性能或[分配 pod 优先级](cs_pod_priority.html#pod_priority)。</td>
</tr>
</tbody></table>

## 2018 年 7 月的热门主题
{: #july18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 7 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>7 月 30 日</td>
<td>[自带 Ingress 控制器](cs_ingress.html#user_managed)</td>
<td>您对集群的 Ingress 控制器有非常具体的安全性或其他定制需求吗？如果有，那么您可能希望运行自己的 Ingress 控制器，而不是缺省 Ingress 控制器。</td>
</tr>
<tr>
<td>7 月 10 日</td>
<td>引入多专区集群</td>
<td>想要提高集群可用性吗？现在，您可以使集群跨精选大城市区域中的多个专区。有关更多信息，请参阅[在 {{site.data.keyword.containerlong_notm}} 中创建多专区集群](cs_clusters_planning.html#multizone)。</td>
</tr>
</tbody></table>

## 2018 年 6 月的热门主题
{: #june18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 6 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>6 月 13 日</td>
<td>`bx` CLI 命令名称将更改为 `ic` CLI</td>
<td>下载最新版本的 {{site.data.keyword.Bluemix_notm}} CLI 后，现在是使用 `ic` 前缀而不是 `bx` 来运行命令。例如，通过运行 `ibmcloud ks clusters` 列出集群。</td>
</tr>
<tr>
<td>6 月 12 日</td>
<td>[pod 安全策略](cs_psp.html)</td>
<td>对于运行 Kubernetes 1.10.3 或更高版本的集群，可以配置 pod 安全策略以授权谁可以在 {{site.data.keyword.containerlong_notm}} 中创建和更新 pod。</td>
</tr>
<tr>
<td>6 月 6 日</td>
<td>[对 IBM 提供的 Ingress 通配符子域的 TLS 支持](cs_ingress.html#wildcard_tls)</td>
<td>对于在 2018 年 6 月 6 日或之后创建的集群，现在 IBM 提供的 Ingress 子域 TLS 证书是通配符证书，可用于已注册的通配符子域。对于在 2018 年 6 月 6 日之前创建的集群，在更新当前 TLS 证书时，TLS 证书会更新为通配符证书。</td>
</tr>
</tbody></table>

## 2018 年 5 月的热门主题
{: #may18}


<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 5 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>5 月 24 日</td>
<td>[新的 Ingress 子域格式](cs_ingress.html)</td>
<td>对于 5 月 24 日之后创建的集群，将为其分配新格式的 Ingress 子域：<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>。使用 Ingress 公开应用程序时，可以使用新的子域从因特网访问应用程序。</td>
</tr>
<tr>
<td>5 月 14 日</td>
<td>[更新：在全球范围内的 GPU 裸机上部署工作负载](cs_app.html#gpu_app)</td>
<td>如果集群中有[裸机图形处理单元 (GPU) 机器类型](cs_clusters_planning.html#shared_dedicated_node)，那么可以安排数学密集型应用程序。GPU 工作程序节点可以处理 CPU 和 GPU 上的应用程序工作负载以提高性能。</td>
</tr>
<tr>
<td>5 月 3 日</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>您的团队需要一些额外的帮助来了解要在应用程序容器中拉入哪个映像吗？请试用 Container Image Security Enforcement (beta)，以便在部署容器映像之前验证这些映像。可用于运行 Kubernetes 1.9 或更高版本的集群。</td>
</tr>
<tr>
<td>5 月 1 日</td>
<td>[通过 GUI 部署 Kubernetes 仪表板](cs_app.html#cli_dashboard)</td>
<td>您曾想过仅单击一次就访问 Kubernetes 仪表板吗？请使用 {{site.data.keyword.Bluemix_notm}} GUI 中的 **Kubernetes 仪表板**按钮。</td>
</tr>
</tbody></table>




## 2018 年 4 月的热门主题
{: #apr18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 4 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>4 月 17 日</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>安装 {{site.data.keyword.Bluemix_notm}} Block Storage [插件](cs_storage_block.html#install_block)以在块存储器中保存持久数据。然后，可以为集群[创建新的](cs_storage_block.html#add_block)块存储器或[使用现有](cs_storage_block.html#existing_block)块存储器。</td>
</tr>
<tr>
<td>4 月 13 日</td>
<td>[用于将 Cloud Foundry 应用程序迁移到集群的新教程](cs_tutorials_cf.html#cf_tutorial)</td>
<td>您有 Cloud Foundry 应用程序吗？请了解如何将该应用程序中的相同代码部署到在 Kubernetes 集群中运行的容器。</td>
</tr>
<tr>
<td>4 月 5 日</td>
<td>[过滤日志](cs_health.html#filter-logs)</td>
<td>过滤掉特定日志，使其不会转发。可以针对特定名称空间、容器名称、日志级别和消息字符串过滤掉日志。</td>
</tr>
</tbody></table>

## 2018 年 3 月的热门主题
{: #mar18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 3 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>3 月 16 日</td>
<td>[供应具有可信计算的裸机集群](cs_clusters_planning.html#shared_dedicated_node)</td>
<td>创建运行 [Kubernetes V1.9](cs_versions.html#cs_v19) 或更高版本的裸机集群，并启用“可信计算”来验证工作程序节点是否被篡改。</td>
</tr>
<tr>
<td>3 月 14 日</td>
<td>[使用 {{site.data.keyword.appid_full}} 安全登录](cs_integrations.html#appid)</td>
<td>通过要求用户登录，增强了在 {{site.data.keyword.containerlong_notm}} 中运行的应用程序的功能。</td>
</tr>
<tr>
<td>3 月 13 日</td>
<td>[在圣保罗提供了专区](cs_regions.html)</td>
<td>欢迎巴西圣保罗成为美国南部区域的一个新专区。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>3 月 12 日</td>
<td>[刚刚使用试用帐户加入 {{site.data.keyword.Bluemix_notm}}？请试用免费 Kubernetes 集群！](container_index.html#clusters)</td>
<td>通过试用 [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/registration/)，您可以部署一个免费集群并使用 30 天，以测试 Kubernetes 功能。</td>
</tr>
</tbody></table>

## 2018 年 2 月的热门主题
{: #feb18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 2 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td>2 月 27 日</td>
<td>工作程序节点的硬件虚拟机 (HVM) 映像</td>
<td>通过 HVM 映像来提高工作负载的 I/O 性能。使用 `ibmcloud ks worker-reload` [命令](cs_cli_reference.html#cs_worker_reload)或 `ibmcloud ks worker-update` [命令](cs_cli_reference.html#cs_worker_update)在每个现有工作程序节点上进行激活。</td>
</tr>
<tr>
<td>2 月 26 日</td>
<td>[KubeDNS 自动扩展](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>现在，KubeDNS 可随着集群的增长而扩展。可以使用以下命令来调整扩展配额：`kubectl -n kube-system edit cm kube-dns-autoscaler`。</td>
</tr>
<tr>
<td>2 月 23 日</td>
<td>在 Web UI 中查看[日志记录](cs_health.html#view_logs)和[度量值](cs_health.html#view_metrics)</td>
<td>通过改进的 Web UI，轻松查看集群及其组件的日志和度量数据。有关访问权的信息，请参阅“集群详细信息”页面。</td>
</tr>
<tr>
<td>2 月 20 日</td>
<td>加密映像和[签名的可信内容](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>在 {{site.data.keyword.registryshort_notm}} 中，可以对映像进行签名和加密，以确保这些映像在注册表名称空间中存储时的完整性。仅使用可信内容来运行容器实例。</td>
</tr>
<tr>
<td>2 月 19 日</td>
<td>[设置 strongSwan IPSec VPN](cs_vpn.html#vpn-setup)</td>
<td>快速部署 strongSwan IPSec VPN Helm 图表，无需虚拟路由器设备就能将 {{site.data.keyword.containerlong_notm}} 集群安全地连接到内部部署数据中心。</td>
</tr>
<tr>
<td>2 月 14 日</td>
<td>[在首尔提供了专区](cs_regions.html)</td>
<td>将 Kubernetes 集群部署到亚太地区北部区域的首尔，刚好赶上奥运会。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于此专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>2 月 8 日</td>
<td>[更新 Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>在更新到 Kubernetes 1.9 之前，请查看要对集群进行的更改。</td>
</tr>
</tbody></table>

## 2018 年 1 月的热门主题
{: #jan18}

<table summary="此表显示热门主题。各行都应从左到右阅读，其中第一列是日期，第二列是功能标题，第三列是描述。">
<caption>2018 年 1 月容器和 Kubernetes 集群的热门主题</caption>
<thead>
<th>日期</th>
<th>标题</th>
<th>描述</th>
</thead>
<tbody>
<td>1 月 25 日</td>
<td>[提供了全局注册表](../services/Registry/registry_overview.html#registry_regions)</td>
<td>通过 {{site.data.keyword.registryshort_notm}}，可以使用全局 `registry.bluemix.net` 来拉取 IBM 提供的公共映像。</td>
</tr>
<tr>
<td>1 月 23 日</td>
<td>[在新加坡和加拿大蒙特利尔提供了专区](cs_regions.html)</td>
<td>新加坡和蒙特利尔是 {{site.data.keyword.containerlong_notm}} 亚太地区北部和美国东部区域中可用的专区。如果您有防火墙，请确保[打开防火墙端口](cs_firewall.html#firewall)以用于这些专区以及您集群所在区域内的其他专区。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[提供增强的类型模板](cs_cli_reference.html#cs_machine_types)</td>
<td>系列 2 虚拟机类型包括本地 SSD 存储和磁盘加密。[迁移工作负载](cs_cluster_update.html#machine_type)至这些类型模板，以提高性能和稳定性。</td>
</tr>
</tbody></table>

## 在 Slack 上与志趣相投的开发人员交谈
{: #slack}

您可以在 [{{site.data.keyword.containerlong_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中查看其他人正在讨论的内容并提出自己的问题。
{:shortdesc}

如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
{: tip}
