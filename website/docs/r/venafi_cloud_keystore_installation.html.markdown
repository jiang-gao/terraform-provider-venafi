---
subcategory: "Cloud Provisioning"
layout: "venafi"
page_title: "Venafi: venafi_cloud_keystore_installation"
description: |-
  Provisions a Venafi managed certificate to an existing Cloud Keystore in Venafi Control Plane (VCP)
---

# Resource: venafi_cloud_keystore_installation

Provisions a certificate from Venafi Control Plane's inventory to any of the supported Cloud Providers: Amazon 
Certificate Manager, Azure KeyVault or Google Certificate Manager. Exports the ID of the provisioned certificate: 
certificate name for AKV and GCM or ARN for ACM.

## Example Usage

```hcl
# Provision a certificate to cloud keystore with static values
resource "venafi_cloud_keystore_installation" "ck_installation_example_by_id" {
  cloud_keystore_id = "e48897d0-2762-11ef-198k-79ac590dd358"
  certificate_id = "1877af16-2762-11ef-8fab-cc123456ff7"
  cloud_certificate_name = "com-terraform-example-com"
}

# Provision a certificate to cloud keystore
resource "venafi_cloud_keystore_installation" "ck_installation_example" {
  cloud_keystore_id = data.venafi_cloud_keystore.ck_example.id
  certificate_id = venafi_certificate.certificate_example.certificate_id
  cloud_certificate_name = venafi_certificate.certificate_example.common_name
}
```

## Argument Reference

* `cloud_keystore_id` - (Required, string) ID of the cloud keystore where the certificate will be provisioned.
* `certificate_id` - (Required, string) ID of the certificate to be provisioned to the given `keystore_id`.
* `cloud_certificate_name` - (Optional, string) Name for the provisioned certificate in the keystore. If the name already exists, the provisioning will replace the previous certificate with the one from `certificate_id`. Only valid for AKV and GCM keystores.
* `arn` - (Optional, string) ARN of the AWS certificate. Use it to provision the VCP certificate to an existing ACM certificate, instead of a new one. Only valid for ACM keystores.
* `gcm_cert_scope` - (Optional, string) The GCM certificate scope of the certificate. Only valid for GCM keystores. Supported values from GCM API documentation: DEFAULT, EDGE_CACHE, ALL_REGIONS.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `cloud_certificate_id` - The ID of the provisioned certificate within the Cloud Keystore
* `cloud_certificate_metadata` - Metadata of the provisioned certificate from the Cloud Keystore

## Import

Using `terraform import`, import a Machine Identity from Venafi Control Plane using their ID. For example:

```console
terraform import venafi_cloud_keystore_installation.example 2155bd32-2234-22ac-7cfd-ff1198845aa2
```
