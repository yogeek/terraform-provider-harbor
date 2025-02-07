# Resource: harbor_replication



## Example Usage

```hcl
resource "harbor_registry" "main" {
  provider_name = "docker-hub"
  name          = "test_docker_harbor"
  endpoint_url  = "https://hub.docker.com"

}


resource "harbor_replication" "push" {
  name        = "test_push"
  action      = "push"
  registry_id = harbor_registry.main.registry_id
}

resource "harbor_replication" "alpine" {
  name        = "alpine"
  action      = "pull"
  registry_id = harbor_registry.main.registry_id
  schedule = "* 0/15 * * * *"
  filters {
    name = "library/alpine"
  }
  filters {
    tag = "3.*.*"
  }
  filters {
    resource = "artifact"
  }
}
 
```

## Argument Reference
The following arguments are supported:

* **name** - (Required)

* **action** - (Required)

* **schedule** - (Optional) The scheduled time of when the container register will be push / pull. In cron base format. Hourly `"0 0 * * * *"`, Daily `"0 0 0 * * *"`, Monthly `"0 0 0 * * 0"`
* **override** - (Optional) Specify whether to override the resources at the destination if a resources with the same name exist. Can be set to `true` or `false` (Default: `true`)
* **enabled** - (Optional) Specify whether the replication is enabled. Can be set to `true` or `false` (Default: `true`)
* **description** - (Optional) Write about description of the replication policy.
* **dest_namespace** - (Optional) Specify the destination namespace. if empty, the resource will be put under the same namespace as the source.
* **deletion** - (Optional) Specify whether to delete the remote resources when locally deleted. Can be set to `true` or `false` (Default: `false`)

* **filters** - (Optional) A collection of `filters` block as documented below.

---

**filters** supports the following:

* **name** - (Optional) Filter on the name of the resource.
* **tag** - (Optional) Filter on the tag/version of the resource.
* **labels** - (Optional) Filter on the resource according to labels.
* **resource** - (Optional) Filter on the resource type. Can be one of the following types. `chart`, `artifact`
				


## Attributes Reference
In addition to all argument, the following attributes are exported:

* **replication_policy_id**
  
## Import
Harbor project can be imported using the `replication id` eg,

`
terraform import harbor_project.main /registries/7
`
