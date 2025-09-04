# Terraform Cheatsheet - Table Format

[![IaC](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)](https://terraform.io)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-FFDD00?logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/dcorneschi)

## Installation Commands

| Platform | Method | Command |
|----------|--------|---------|
| **Ubuntu/Debian** | Package Manager | `wget -O - https://apt.releases.hashicorp.com/gpg \| sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg`<br>`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release \|\| lsb_release -cs) main" \| sudo tee /etc/apt/sources.list.d/hashicorp.list`<br>`sudo apt update && sudo apt install terraform` |
| **CentOS/RHEL** | Package Manager | `sudo yum install -y yum-utils`<br>`sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo`<br>`sudo yum -y install terraform` |
| **Amazon Linux** | Package Manager | `sudo yum install -y yum-utils shadow-utils`<br>`sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo`<br>`sudo yum -y install terraform` |
| **macOS** | Homebrew | `brew tap hashicorp/tap`<br>`brew install hashicorp/tap/terraform` |
| **macOS (Intel)** | Manual | `curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_amd64.zip`<br>`unzip terraform_1.13.0_darwin_amd64.zip`<br>`sudo mv terraform /usr/local/bin/` |
| **macOS (Apple Silicon)** | Manual | `curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_arm64.zip`<br>`unzip terraform_1.13.0_darwin_arm64.zip`<br>`sudo mv terraform /usr/local/bin/` |
| **All Platforms** | Autocomplete | `terraform -install-autocomplete` |

## Core Commands

| Category | Command | Description |
|----------|---------|-------------|
| **Version** | `terraform version` | Get the terraform version |
| **Initialization** | `terraform init` | Initialize configuration (generate .terraform & .terraform.lock.hcl) |
| **Initialization** | `terraform init -backend-config="key=value"` | Initialize with backend configuration |
| **Initialization** | `terraform init -backend-config="backend.conf"` | Initialize with backend configuration file |
| **Initialization** | `terraform init -backend=false` | Skip backend initialization entirely |
| **Initialization** | `terraform init -upgrade` | Initialize and upgrade providers |
| **Initialization** | `terraform init -upgrade=hashicorp/aws` | Upgrade only specific providers |
| **Initialization** | `terraform init -get-plugins=false` | Initialize without plugins (offline mode) |
| **Initialization** | `terraform init -get=false` | Skip getting/updating modules |
| **Initialization** | `terraform init -reconfigure` | Reconfigure backend ignoring saved configuration |
| **Initialization** | `terraform init -migrate-state` | Migrate state from another backend |
| **Initialization** | `terraform init -input=false` | Skip interactive prompts |
| **Initialization** | `terraform init -verify-plugins=false` | Skip plugin signature verification |
| **Initialization** | `terraform init -lock=false` | Don't hold state lock during migration (dangerous) |
| **Initialization** | `terraform init -lock-timeout=120s` | Change state lock timeout |

## Authentication

| Command | Description |
|---------|-------------|
| `terraform login` | Log in to Terraform Cloud |
| `terraform logout` | Log out from Terraform Cloud |
| `terraform login <hostname>` | Log in to a different host |

## Module Management

| Command | Description |
|---------|-------------|
| `terraform get` | Get modules |
| `terraform get -update` | Update modules |
| `terraform graph -type=plan \| grep module` | Show module tree |

## Planning & Validation

| Category | Command | Description |
|----------|---------|-------------|
| **Planning** | `terraform plan` | Create an execution plan |
| **Planning** | `terraform plan -out=tfplan` | Save plan to a file |
| **Planning** | `terraform plan -var-file="prod.tfvars"` | Plan with variable files |
| **Planning** | `terraform plan -var="region=us-west-2"` | Plan with inline variables |
| **Planning** | `terraform plan -replace aws_instance.example` | Replace selected resources |
| **Planning** | `terraform plan -target=module.vpc` | Target specific resources |
| **Validation** | `terraform validate` | Validate configuration files |
| **Formatting** | `terraform fmt` | Format configuration files |
| **Formatting** | `terraform fmt -check` | Check if input is formatted |
| **Formatting** | `terraform fmt -diff` | Display formatting differences |
| **Formatting** | `terraform fmt -recursive` | Format files in subdirectories |

## Apply & Deploy

| Command | Description |
|---------|-------------|
| `terraform apply` | Apply changes |
| `terraform apply tfplan` | Apply a saved plan |
| `terraform apply -auto-approve` | Auto-approve without confirmation |
| `terraform apply -target=aws_instance.example` | Apply with specific targets |
| `terraform apply -var="key=value"` | Apply with specific variable |
| `terraform apply -var-file="prod.tfvars"` | Apply with variable files |
| `terraform apply --parallelism=5` | Set number of simultaneous operations |

## Destroy & Cleanup

| Command | Description |
|---------|-------------|
| `terraform destroy` | Destroy all resources |
| `terraform destroy -auto-approve` | Destroy with auto-approve |
| `terraform destroy -target=aws_instance.example` | Destroy specific resources |
| `terraform plan -destroy` | Plan destruction |
| `terraform plan -destroy -out=destroy.tfplan` | Save destroy plan to file |
| `terraform apply destroy.tfplan` | Apply destroy plan |

## State Management

| Command | Description |
|---------|-------------|
| `terraform show` | Show current state |
| `terraform show my.plan` | Inspect the plan |
| `terraform state list` | List resources in state |
| `terraform state list \| wc -l` | Count resources |
| `terraform state list \| cut -d. -f1 \| sort \| uniq -c` | Count resources by type |
| `terraform show -json \| jq '.values.root_module.resources[] \| {address: .address, depends_on: .depends_on}'` | Get all resource dependencies |
| `terraform state show aws_instance.example` | Show specific resource |
| `terraform state rm aws_instance.example` | Remove resource from state |
| `terraform import aws_instance.example i-1234567890abcdef0` | Import existing resource |
| `terraform state mv aws_instance.example aws_instance.new_name` | Move resource in state |
| `terraform state pull` | Pull remote state |
| `terraform state pull > terraform.tfstate` | Save remote state to file |
| `terraform state push terraform.tfstate` | Push local state to remote |
| `terraform force-unlock -force my_lock_id` | Manually unlock state |

## Workspace Management

| Command | Description |
|---------|-------------|
| `terraform workspace list` | List workspaces |
| `terraform workspace new dev` | Create new workspace |
| `terraform workspace select prod` | Select workspace |
| `terraform workspace show` | Show current workspace |
| `terraform workspace delete dev` | Delete workspace |

## Output & Variables

| Command | Description |
|---------|-------------|
| `terraform output` | Show outputs |
| `terraform output instance_ip` | Show specific output |
| `terraform output -json` | Show outputs in JSON format |
| `terraform output -raw <name>` | Show specific output value without quotes |

## Debugging & Troubleshooting

| Log Level | Command | Description |
|-----------|---------|-------------|
| **Debug** | `export TF_LOG=DEBUG; terraform plan` | Enable debug logging |
| **Debug** | `TF_LOG=DEBUG terraform init` | Enable debug logging for specific command |
| **File Logging** | `export TF_LOG_PATH=./terraform.log; terraform apply` | Log to file |
| **Disable** | `unset TF_LOG` or `export TF_LOG=` | Disable logging |

**Available Log Levels:** TRACE, DEBUG, INFO, WARN, ERROR (in order of decreasing verbosity)

## Terraform Console Functions

| Category | Function | Example |
|----------|----------|---------|
| **String** | `upper()` | `echo 'upper("hello")' \| terraform console` |
| **String** | `join()` | `echo 'join(",",[\"a\",\"b\",\"c\"])' \| terraform console` |
| **String** | `split()` | `echo 'split(",","a,b,c")' \| terraform console` |
| **String** | `replace()` | `echo 'replace("hello world", "world", "terraform")' \| terraform console` |
| **List** | `length()` | `echo 'length([\"a\",\"b\",\"c\"])' \| terraform console` |
| **List** | `element()` | `echo 'element([\"a\",\"b\",\"c\"], 1)' \| terraform console` |
| **List** | `contains()` | `echo 'contains([\"a\",\"b\",\"c\"], \"b\")' \| terraform console` |
| **Encoding** | `base64encode()` | `echo 'base64encode("hello")' \| terraform console` |
| **Encoding** | `base64decode()` | `echo 'base64decode("aGVsbG8=")' \| terraform console` |
| **Encoding** | `jsonencode()` | `echo 'jsonencode({name="test"})' \| terraform console` |

## Graph & Dependencies

| Command | Description |
|---------|-------------|
| `terraform graph` | Generate dependency graph |
| `terraform graph > graph.dot` | Save graph to file |
| `terraform graph \| dot -Tpng > graph.png` | Convert to PNG (requires Graphviz) |
| `terraform graph -draw-cycles` | Show resource dependencies |

## Provider Management

| Command | Description |
|---------|-------------|
| `terraform providers` | Show providers |
| `terraform providers lock` | Lock provider versions |
| `terraform providers lock -platform=linux_amd64 -platform=darwin_amd64 -platform=windows_amd64` | Pre-populate hashes for multiple platforms |
| `terraform providers mirror /path/to/mirror` | Mirror providers for offline use |

## State File Operations

| Command | Description |
|---------|-------------|
| `cp terraform.tfstate terraform.tfstate.backup` | Backup state file |
| `cp terraform.tfstate.backup terraform.tfstate` | Restore state file |
| `terraform show -json > state.json` | Convert state to JSON |
| `diff terraform.tfstate.backup terraform.tfstate` | Compare state files |
| `terraform plan -refresh-only` | Inspect resource drift without updating state |
| `terraform apply -refresh-only` | Plan and refresh the state file |

## Testing & Validation

| Command | Description |
|---------|-------------|
| `terraform validate` | Validate syntax (requires terraform init) |
| `terraform validate -backend=false` | Validate code, skip backend validation |
| `terraform validate -json` | Output in machine-readable JSON format |

## Utility Tools

### tfenv (Terraform Version Manager)

| Platform | Command | Description |
|----------|---------|-------------|
| **macOS** | `brew install tfenv` | Install tfenv |
| **Linux** | `git clone --depth=1 https://github.com/tfutils/tfenv.git ~/.tfenv`<br>`echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bash_profile` | Install tfenv |
| **All** | `tfenv list-remote` | List available versions |
| **All** | `tfenv install 1.5.0` | Install specific version |
| **All** | `tfenv use 1.5.0` | Use specific version |
| **All** | `tfenv install latest` | Install latest version |
| **All** | `tfenv version` | Show current version |

### tflint (Terraform Linter)

| Platform | Command | Description |
|----------|---------|-------------|
| **macOS** | `brew install tflint` | Install tflint |
| **Linux** | `curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh \| bash` | Install tflint |
| **All** | `tflint` | Lint with TFLint |

### tfsec (Security Scanner)

| Platform | Command | Description |
|----------|---------|-------------|
| **macOS** | `brew install tfsec` | Install tfsec |
| **Linux** | `curl -s https://raw.githubusercontent.com/aquasecurity/tfsec/master/scripts/install_linux.sh \| bash` | Install tfsec |
| **All** | `tfsec .` | Scan for security issues |
| **All** | `tfsec ./infrastructure` | Scan specific directory |
| **All** | `tfsec --format json .` | Output JSON format |

### terraform-docs (Documentation Generator)

| Platform | Command | Description |
|----------|---------|-------------|
| **macOS** | `brew install terraform-docs` | Install terraform-docs |
| **Linux** | `curl -sSLo ./terraform-docs.tar.gz https://terraform-docs.io/dl/v0.20.0/terraform-docs-v0.20.0-$(uname)-amd64.tar.gz`<br>`tar -xzf terraform-docs.tar.gz`<br>`chmod +x terraform-docs`<br>`mv terraform-docs /some-dir-in-your-PATH/terraform-docs` | Install terraform-docs |
| **All** | `terraform-docs markdown . > README.md` | Generate documentation |

## Environment Variables

| Variable | Example | Description |
|----------|---------|-------------|
| `TF_VAR_region` | `export TF_VAR_region="us-west-2"` | Set Terraform variable |
| `TF_VAR_instance_type` | `export TF_VAR_instance_type="t3.micro"` | Set Terraform variable |
| `TF_LOG` | `export TF_LOG=DEBUG` | Set logging level |
| `TF_LOG_PATH` | `export TF_LOG_PATH="./terraform.log"` | Set log file path |
| `TF_CLI_CONFIG_FILE` | `export TF_CLI_CONFIG_FILE="$HOME/.terraformrc"` | Set CLI config file |

## State Management Best Practices

| Practice | Command | Description |
|----------|---------|-------------|
| **Backup** | `cp terraform.tfstate terraform.tfstate.backup.$(date +%Y%m%d_%H%M%S)` | Backup state before major operations |
| **Remote State** | `terraform init -backend-config="bucket=my-tf-state" -backend-config="key=prod/terraform.tfstate"` | Use remote state |
| **State Locking** | `terraform init -backend-config="dynamodb_table=terraform-locks"` | Enable state locking |

## Useful Aliases

Add these to your `~/.zshrc` or `~/.bashrc`:

| Alias | Command | Description |
|-------|---------|-------------|
| `tf` | `terraform` | Short for terraform |
| `tfi` | `terraform init` | Initialize |
| `tfp` | `terraform plan` | Plan |
| `tfa` | `terraform apply` | Apply |
| `tfd` | `terraform destroy` | Destroy |
| `tfs` | `terraform show` | Show |
| `tfo` | `terraform output` | Output |
| `tfv` | `terraform validate` | Validate |
| `tff` | `terraform fmt` | Format |
| `tfw` | `terraform workspace` | Workspace |
| `tfws` | `terraform workspace show` | Show workspace |
| `tfsl` | `terraform state list` | State list |

## Useful Functions

```bash
# Generic function for any base64 string
tf_base64_decode() {
    if [ $# -eq 0 ]; then
        echo "Usage: tf_base64_decode '<base64_string>'"
        return 1
    fi
    echo "base64decode(\"$1\")" | terraform console
}
```

## Best Practices & Tips

| # | Practice |
|---|----------|
| 1 | Always commit lock files to version control alongside your Terraform configuration files |
| 2 | Never add `.terraform.lock.hcl` to `.gitignore` - ignoring lock files defeats their purpose |
| 3 | Never commit .tfvars files with secrets |
| 4 | Store state files remotely and securely |
| 5 | Use workspaces for environment separation |
| 6 | Use modules for reusable components |
| 7 | Pin provider versions in production |
| 8 | Always run `terraform plan` before `apply` |
| 9 | Review and understand the execution plan before applying |

---

<div align="center">

**Made with ❤️ by [Daniel Corneschi](https://github.com/dcorneschi)**

If you enjoyed this project, please consider giving it a ⭐️!

</div>
