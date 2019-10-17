# Google IAM Terraform Module

This Terraform module makes it easier to non-destructively manage multiple IAM roles for resources on Google Cloud Platform.

## Compatibility

This module is meant for use with Terraform 0.12. If you haven't
[upgraded][terraform-0.12-upgrade] and need a Terraform 0.11.x-compatible
version of this module, the last released version intended for Terraform 0.11.x
is [1.1.1][v1.1.1].

## Upgrading

The following guides are available to assist with upgrades:

- [2.0 -> 3.0](./docs/upgrading_to_iam_3.0.md)

## Usage

Full examples are in the [examples](./examples/) folder, but basic usage is as follows for managing roles on two projects:

```hcl
module "projects_iam_bindings" {
  source  = "terraform-google-modules/iam/google//modules/projects_iam"
  version = "~> 3.0"

  projects = ["project-123456", "project-9876543"]

  bindings = {
    "roles/storage.admin" = [
      "group:test_sa_group@lnescidev.com",
      "user:someone@google.com",
    ]

    "roles/compute.networkAdmin" = [
      "group:test_sa_group@lnescidev.com",
      "user:someone@google.com",
    ]

    "roles/compute.imageUser" = [
      "user:someone@google.com",
    ]
  }
}
```

The module also offers an **authoritative** mode which will remove all roles not assigned through Terraform. This is an example of using the authoritative mode to manage access to a storage bucket:

```hcl
module "storage_buckets_iam_bindings" {
  source  = "terraform-google-modules/iam/google//modules/storage_buckets_iam"
  version = "~> 3.0"

  storage_buckets = ["my-storage-bucket"]

  mode = "authoritative"

  bindings = {
    "roles/storage.legacyBucketReader" = [
      "user:josemanuelt@google.com",
      "group:test_sa_group@lnescidev.com",
    ]

    "roles/storage.legacyBucketWriter" = [
      "user:josemanuelt@google.com",
      "group:test_sa_group@lnescidev.com",
    ]
  }
}
```

### Variables

Following variables are the most important to control module's behavior:

- Mode

    This variable controls the module's behavior, by default is set to "additive", possible options are:

      - additive: add members to role, old members are not deleted from this role.
      - authoritative: set the role's members, other roles' members are not deleted.

- Bindings

  Is a map of role (key) and list of members (value) with member type prefix, for example:

```hcl
        bindings = {
            "roles/<some_role>" = [
                "user:someone@somewhere.com",
                "group:somepeople@somewhereelse.com"
            ]
        }
```

- Project

  This variable must be defined in case of using one the following modules:

  - `pubsub_subscriptions_iam`
  - `pubsub_topics_iam`
  - `service_accounts_iam`
  - `subnets_iam`

- Subnets_region

  This variable must be defined in case of using module `subnets_iam`

#### Additive and Authoritative Modes

This module includes two modes: additive and authoritative.

In authoritative mode, the module takes full control over the IAM bindings listed in the module. This means that any members added to roles outside the module will be removed the next time Terraform runs. However, roles not listed in the module will be unaffected.

In additive mode, this module leaves existing bindings unaffected. Instead, any members listed in the module will be added to the existing set of IAM bindings. However, members listed in the module *are* fully controlled by the module. This means that if you add a binding via the module and later remove it, the module will correctly handle removing the role binding.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| folders | Folders list to add the IAM policies/bindings | list(string) | `<list>` | no |
| folders\_bindings | Map of role (key) and list of members (value) to add the Folders IAM policies/bindings | map(list(string)) | n/a | yes |
| folders\_bindings\_num | Number of Folders bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| folders\_mode | Mode for adding the Folders IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| folders\_num | Number of Folders, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| kms\_crypto\_keys | KMS Crypto Keys list to add the IAM policies/bindings | list(string) | `<list>` | no |
| kms\_crypto\_keys\_bindings | Map of role (key) and list of members (value) to add the KMS Crypto Keys IAM policies/bindings | map(list(string)) | n/a | yes |
| kms\_crypto\_keys\_bindings\_num | Number of KMS Crypto Keys bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| kms\_crypto\_keys\_mode | Mode for adding the KMS Crypto Keys IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| kms\_crypto\_keys\_num | Number of KMS Crypto Keys, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| kms\_key\_rings | KMS Key Rings list to add the IAM policies/bindings | list(string) | `<list>` | no |
| kms\_key\_rings\_bindings | Map of role (key) and list of members (value) to add the KMS Key Rings IAM policies/bindings | map(list(string)) | n/a | yes |
| kms\_key\_rings\_bindings\_num | Number of KMS Key Rings bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| kms\_key\_rings\_mode | Mode for adding the KMS Key Rings IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| kms\_key\_rings\_num | Number of KMS Key Rings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| organizations | Organizations list to add the IAM policies/bindings | list(string) | `<list>` | no |
| organizations\_bindings | Map of role (key) and list of members (value) to add the Organizations IAM policies/bindings | map(list(string)) | n/a | yes |
| organizations\_bindings\_num | Number of Organizations bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| organizations\_mode | Mode for adding the Organizations IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| organizations\_num | Number of Organizations, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| project | Project to add the IAM policies/bindings | string | `""` | no |
| projects | Projects list to add the IAM policies/bindings | list | `<list>` | no |
| projects\_bindings | Map of role (key) and list of members (value) to add the Projects IAM policies/bindings | map(list(string)) | n/a | yes |
| projects\_bindings\_num | Number of Projects bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| projects\_mode | Mode for adding the Projects IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| projects\_num | Number of Projects, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| pubsub\_subscriptions | PubSub Subscriptions list to add the IAM policies/bindings | list(string) | `<list>` | no |
| pubsub\_subscriptions\_bindings | Map of role (key) and list of members (value) to add the PubSub Subscriptions IAM policies/bindings | map(list(string)) | n/a | yes |
| pubsub\_subscriptions\_bindings\_num | Number of PubSub Subscriptions bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| pubsub\_subscriptions\_mode | Mode for adding the PubSub Subscriptions IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| pubsub\_subscriptions\_num | Number of PubSub Subscriptions, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| pubsub\_topics | PubSub Topics list to add the IAM policies/bindings | list(string) | `<list>` | no |
| pubsub\_topics\_bindings | Map of role (key) and list of members (value) to add the PubSub Topics IAM policies/bindings | map(list(string)) | n/a | yes |
| pubsub\_topics\_bindings\_num | Number of PubSub Topics bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| pubsub\_topics\_mode | Mode for adding the PubSub Topics IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| pubsub\_topics\_num | Number of PubSub Topics, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| service\_accounts | Service Accounts list to add the IAM policies/bindings | list(string) | `<list>` | no |
| service\_accounts\_bindings | Map of role (key) and list of members (value) to add the Service Accounts IAM policies/bindings | map(list(string)) | n/a | yes |
| service\_accounts\_bindings\_num | Number of Service Accounts bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| service\_accounts\_mode | Mode for adding the Service Accounts IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| service\_accounts\_num | Number of Service Accounts, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| storage\_buckets | Storage Buckets list to add the IAM policies/bindings | list(string) | `<list>` | no |
| storage\_buckets\_bindings | Map of role (key) and list of members (value) to add the Storage Buckets IAM policies/bindings | map(list(string)) | n/a | yes |
| storage\_buckets\_bindings\_num | Number of Storage Buckets bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| storage\_buckets\_mode | Mode for adding the Storage Buckets IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| storage\_buckets\_num | Number of Storage Buckets, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| subnets | Subnets list to add the IAM policies/bindings | list(string) | `<list>` | no |
| subnets\_bindings | Map of role (key) and list of members (value) to add the Subnets IAM policies/bindings | map(list(string)) | n/a | yes |
| subnets\_bindings\_num | Number of Subnets bindings, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| subnets\_mode | Mode for adding the Subnets IAM policies/bindings, additive and authoritative | string | `"additive"` | no |
| subnets\_num | Number of Subnets, in case using dependcies of outher resources's outputs | number | `"0"` | no |
| subnets\_region | Subnets region | string | n/a | yes |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Caveats

### Referencing values/attributes from other resources

This Terraform module performs operations over some variables before making any changes on the IAM bindings in GCP.

The following variables have associated `*_num` variables which must be jointly configured with the number of elements:

- `projects`
- `organizations`
- `folders`
- `service_accounts`
- `subnets`
- `storage_buckets`
- `pubsub_topics`
- `pubsub_subscriptions`
- `kms_key_rings`
- `kms_crypto_keys`
- `projects_bindings`
- `organizations_bindings`
- `folders_bindings`
- `service_accounts_bindings`
- `subnets_bindings`
- `storage_buckets_bindings`
- `pubsub_topics_bindings`
- `pubsub_subscriptions_bindings`
- `kms_key_rings_bindings`
- `kms_crypto_keys_bindings`

* For `authoritative` mode set variable equals to the number of roles applyed
* For `additive` mode set variable equals to the number of Service Accounts and users and groups applyed

For example, `authoritative` mode:

```hcl
resource google_folder "my_new_folder" {
  display_name = "folder-test"
  parent       = "76543265432"
}

resource "google_service_account" "my_service_account" {
  account_id = "my-new-service-account"
}

module "folders_iam_bindings" {
  source  = "terraform-google-modules/iam/google//modules/folders_iam"
  version = "~> 3.0"

  mode = "authoritative"

  folders     = [google_folder.my_new_folder.id]
  folders_num = 1

  bindings = {
    "roles/storage.admin" = [
      "group:test_sa_group@lnescidev.com",
      "serviceAccount:${google_service_account.my_service_account.id}",
    ]

    "roles/compute.networkAdmin" = [
      "group:test_sa_group@lnescidev.com",
      "user:someone@google.com",
    ]
  }

  bindings_num = 2
}
```

`additive` mode:

```hcl
resource google_folder "my_new_folder" {
  display_name = "folder-test"
  parent       = "76543265432"
}

resource "google_service_account" "my_service_account" {
  account_id = "my-new-service-account"
}

module "folders_iam_bindings" {
  source  = "terraform-google-modules/iam/google//modules/folders_iam"
  version = "~> 3.0"

  mode = "additive"

  folders      = [google_folder.my_new_folder.id]
  folders_num  = 1

  bindings = {
    "roles/storage.admin" = [
      "group:test_sa_group@lnescidev.com",
      "serviceAccount:${google_service_account.my_service_account.id}",
    ]

    "roles/compute.networkAdmin" = [
      "group:test_sa_group@lnescidev.com",
      "user:someone@google.com",
    ]
  }

  bindings_num = 4
}
```

## IAM Bindings

You can choose the following resource types for apply the IAM bindings:

- Projects (`projects` variable)
- Organizations(`organizations` variable)
- Folders (`folders` variable)
- Service Accounts (`service_accounts` variable)
- Subnetworks (`subnets` variable)
- Storage buckets (`storage_buckets` variable)
- Pubsub topics (`pubsub_topics` variable)
- Pubsub subscriptions (`pubsub_subscriptions` variable)
- Kms Key Rings (`kms_key_rings` variable)
- Kms Crypto Keys (`kms_crypto_keys` variable)

Set the specified variable on the module call to choose the resources to affect. Remember to set the `mode` [variable](#variables) and give enough [permissions](#permissions) to manage the selected resource as well.

## Requirements

### Terraform plugins

- [Terraform](https://www.terraform.io/downloads.html) 0.12
- [terraform-provider-google](https://github.com/terraform-providers/terraform-provider-google) 2.5
- [terraform-provider-google-beta](https://github.com/terraform-providers/terraform-provider-google-beta) 2.5

### Permissions

In order to execute this module you must have a Service Account with an appropriate role to manage IAM for the applicable resource. The appropriate role differs depending on which resource you are targeting, as follows:

- Organization:
  - Organization Administrator: Access to administer all resources belonging to the organization
    and does not include privileges for billing or organization role administration.
  - Custom: Add resourcemanager.organizations.getIamPolicy and
    resourcemanager.organizations.setIamPolicy permissions.
- Project:
  - Owner: Full access and all permissions for all resources of the project.
  - Projects IAM Admin: allows users to administer IAM policies on projects.
  - Custom: Add resourcemanager.projects.getIamPolicy and resourcemanager.projects.setIamPolicy permissions.
- Folder:
  - The Folder Admin: All available folder permissions.
  - Folder IAM Admin: Allows users to administer IAM policies on folders.
  - Custom: Add resourcemanager.folders.getIamPolicy and
    resourcemanager.folders.setIamPolicy permissions (must be added in the organization).
- Service Account:
  - Service Account Admin: Create and manage service accounts.
  - Custom: Add resourcemanager.organizations.getIamPolicy and
    resourcemanager.organizations.setIamPolicy permissions.
- Subnetwork:
  - Project compute admin: Full control of Compute Engine resources.
  - Project compute network admin: Full control of Compute Engine networking resources.
  - Project custom: Add compute.subnetworks.getIamPolicy	and
    compute.subnetworks.setIamPolicy permissions.
- Storage bucket:
  - Storage Admin: Full control of GCS resources.
  - Storage Legacy Bucket Owner: Read and write access to existing
    buckets with object listing/creation/deletion.
  - Custom: Add storage.buckets.getIamPolicy	and
  storage.buckets.setIamPolicy permissions.
- Pubsub topic:
  - Pub/Sub Admin: Create and manage service accounts.
  - Custom: Add pubsub.topics.getIamPolicy and pubsub.topics.setIamPolicy permissions.
- Pubsub subscription:
  - Pub/Sub Admin role: Create and manage service accounts.
  - Custom role: Add pubsub.subscriptions.getIamPolicy and
    pubsub.subscriptions.setIamPolicy permissions.
- Kms Key Ring:
  - Owner: Full access to all resources.
  - Cloud KMS Admin: Enables management of crypto resources.
  - Custom: Add cloudkms.keyRings.getIamPolicy and cloudkms.keyRings.getIamPolicy permissions.
- Kms Crypto Key:
  - Owner: Full access to all resources.
  - Cloud KMS Admin: Enables management of cryptoresources.
  - Custom: Add cloudkms.cryptoKeys.getIamPolicy	and cloudkms.cryptoKeys.setIamPolicy permissions.

## Install

### Terraform

Be sure you have the correct Terraform version (0.12), you can choose the binary here:
- https://releases.hashicorp.com/terraform/

### Terraform plugins

Be sure you have the compiled plugins on $HOME/.terraform.d/plugins/

- [terraform-provider-google](https://github.com/terraform-providers/terraform-provider-google) 1.20.0
- [terraform-provider-google-beta](https://github.com/terraform-providers/terraform-provider-google-beta) 1.20.0

See each plugin page for more information about how to compile and use them.

## Fast install (optional)

For a fast install, please configure the variables on init_centos.sh  or init_debian.sh script and then launch it.

The script will do:
- Environment variables setting
- Installation of base packages like wget, curl, unzip, gcloud, etc.
- Installation of go 1.9.0
- Installation of Terraform 0.10.x
- Download the terraform-provider-google plugin
- Compile the terraform-provider-google plugin
- Move the terraform-provider-google to the right location

[v1.1.1]: https://registry.terraform.io/modules/terraform-google-modules/iam/google/1.1.1
[terraform-0.12-upgrade]: https://www.terraform.io/upgrade-guides/0-12.html
