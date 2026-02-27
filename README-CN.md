阿里云 PolarDB AI 搜索 Terraform 模块

================================================ 

# terraform-alicloud-polardb-ai-search

[English](https://github.com/alibabacloud-automation/terraform-alicloud-polardb-ai-search/blob/main/README.md) | 简体中文

这个 Terraform 模块用于创建基于 PolarDB for PostgreSQL 的 AI 驱动多模态智能搜索功能的完整基础设施。该模块实现了[原生 SQL 轻松实现多模态智能检索](https://www.aliyun.com/solution/tech-solution/polardb-ai-search)解决方案架构，涉及专有网络（VPC）、交换机（VSwitch）、PolarDB 数据库（PolarDB）和对象存储服务（OSS）等资源的部署。

## 使用方法

要使用此模块，您需要提供基本的网络配置和数据库凭证。该模块将创建 PolarDB AI 搜索解决方案所需的所有资源。

```terraform
data "alicloud_polardb_node_classes" "default" {
  db_type    = "PostgreSQL"
  db_version = "14"
  pay_type   = "PostPaid"
  category   = "SENormal"
}

data "alicloud_polardb_endpoints" "default" {
  db_cluster_id = module.polardb_ai_search.polardb_cluster_id
}

module "polardb_ai_search" {
  source = "alibabacloud-automation/polardb-ai-search/alicloud"

  # VPC 配置
  vpc_config = {
    cidr_block = "192.168.0.0/16"
    vpc_name   = "polardb-ai-search-vpc"
  }

  # VSwitch 配置
  vswitch_config = {
    cidr_block   = "192.168.1.0/24"
    zone_id      = data.alicloud_polardb_node_classes.default.classes[0].zone_id
    vswitch_name = "polardb-ai-search-vswitch"
  }

  # PolarDB 账户配置
  polardb_account_config = {
    account_name     = "polar_ai"
    account_password = "YourSecurePassword123!"
    account_type     = "Super"
  }

  # PolarDB 端点配置
  polardb_endpoint_config = {
    db_endpoint_id = data.alicloud_polardb_endpoints.default.endpoints[0].db_endpoint_id
    net_type       = "Public"
  }

  # PolarDB 数据库配置
  polardb_database_config = {
    db_name = "ai_search_db"
  }
}
```

## 示例

* [完整示例](https://github.com/alibabacloud-automation/terraform-alicloud-polardb-ai-search/tree/main/examples/complete)

<!-- BEGIN_TF_DOCS -->
<!-- END_TF_DOCS -->

## 提交问题

如果您在使用此模块时遇到任何问题，请提交一个 [provider issue](https://github.com/aliyun/terraform-provider-alicloud/issues/new) 并告知我们。

**注意：** 不建议在此仓库中提交问题。

## 作者

由阿里云 Terraform 团队创建和维护(terraform@alibabacloud.com)。

## 许可证

MIT 许可。有关完整详细信息，请参阅 LICENSE。

## 参考

* [Terraform-Provider-Alicloud Github](https://github.com/aliyun/terraform-provider-alicloud)
* [Terraform-Provider-Alicloud Release](https://releases.hashicorp.com/terraform-provider-alicloud/)
* [Terraform-Provider-Alicloud Docs](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs)