# Terraform Azure Secure VM

Deploys a secure Linux virtual machine on Microsoft Azure using Terraform.
Covers network configuration, security hardening, and infrastructure
provisioning entirely through code.

---

## Infrastructure

- **Resource Group** — scoped deployment container
- **Virtual Network + Subnet** — isolated network (10.0.0.0/16)
- **Network Security Group** — SSH access restricted to a single
  authorised IP address
- **Static Public IP** — Standard SKU
- **Linux VM** — Ubuntu 22.04 LTS, SSH key authentication only,
  no password access

---

## Files

| File | Description |
|---|---|
| `main.tf` | Core infrastructure — VM, network, NSG, public IP |
| `variables.tf` | Input variables for admin username and SSH public key |
| `outputs.tf` | Outputs public IP address and VM name |
| `.gitignore` | Excludes state files, credentials, and provider binaries |

---

## Usage

1. Install [Terraform](https://www.terraform.io/downloads) and
   [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
2. Authenticate: `az login`
3. Initialise: `terraform init`
4. Preview: `terraform plan`
5. Deploy: `terraform apply`

---

## Troubleshooting

### Quota exceeded
Certain VM sizes exceed free tier core quotas. Resolved by selecting
a size within the available subscription quota.

### NSG attachment error
The `network_security_group_id` argument is unsupported directly on
the NIC resource in some provider versions. Resolved by using a
separate `azurerm_network_interface_security_group_association` block.

### SSH connection refused
Caused by NSG rules not permitting port 22, or public IP not yet
assigned. Resolved by verifying NSG inbound rules and confirming
static IP allocation.

### Provider binaries in version control
`.terraform/` directory was committed unintentionally. Resolved by
adding it to `.gitignore` before subsequent pushes.

---

## Stack

Azure · Terraform · Linux · SSH · IAM
