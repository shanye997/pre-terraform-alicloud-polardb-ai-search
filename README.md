Alibaba Cloud PolarDB AI Search Terraform Module

================================================ 

# terraform-alicloud-polardb-ai-search

English | [简体中文](https://github.com/alibabacloud-automation/terraform-alicloud-polardb-ai-search/blob/main/README-CN.md)

This Terraform module creates a complete infrastructure for implementing AI-powered multimodal intelligent search capabilities using PolarDB for PostgreSQL. The module implements the solution architecture described in [Native SQL enables effortless multimodal intelligent search](https://www.aliyun.com/solution/tech-solution/polardb-ai-search), involving the deployment of Virtual Private Cloud (VPC), Virtual Switch (VSwitch), PolarDB Database (PolarDB), and Object Storage Service (OSS) resources.

## Usage

To use this module, you need to provide the basic network configuration and database credentials. The module will create all necessary resources for a PolarDB AI search solution.

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

  # VPC configuration
  vpc_config = {
    cidr_block = "192.168.0.0/16"
    vpc_name   = "polardb-ai-search-vpc"
  }

  # VSwitch configuration
  vswitch_config = {
    cidr_block   = "192.168.1.0/24"
    zone_id      = data.alicloud_polardb_node_classes.default.classes[0].zone_id
    vswitch_name = "polardb-ai-search-vswitch"
  }

  # PolarDB account configuration
  polardb_account_config = {
    account_name     = "polar_ai"
    account_password = "YourSecurePassword123!"
    account_type     = "Super"
  }

  # PolarDB endpoint configuration
  polardb_endpoint_config = {
    db_endpoint_id = data.alicloud_polardb_endpoints.default.endpoints[0].db_endpoint_id
    net_type       = "Public"
  }

  # PolarDB database configuration
  polardb_database_config = {
    db_name = "ai_search_db"
  }
}
```

## Examples

* [Complete Example](https://github.com/alibabacloud-automation/terraform-alicloud-polardb-ai-search/tree/main/examples/complete)

<!-- BEGIN_TF_DOCS -->
<!-- END_TF_DOCS -->

## Submit Issues

If you have any problems when using this module, please opening
a [provider issue](https://github.com/aliyun/terraform-provider-alicloud/issues/new) and let us know.

**Note:** There does not recommend opening an issue on this repo.

## Authors

Created and maintained by Alibaba Cloud Terraform Team(terraform@alibabacloud.com).

## License

MIT Licensed. See LICENSE for full details.

## Reference

* [Terraform-Provider-Alicloud Github](https://github.com/aliyun/terraform-provider-alicloud)
* [Terraform-Provider-Alicloud Release](https://releases.hashicorp.com/terraform-provider-alicloud/)
* [Terraform-Provider-Alicloud Docs](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs)