# Terraform Cheatsheet - Table Format

[![IaC](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)](https://terraform.io)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-FFDD00?logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/dcorneschi)

### Installation Commands

| Platform | Method | Command |
|----------|--------|---------|
| **Ubuntu/Debian** | Package Manager | `wget -O - https://apt.releases.hashicorp.com/gpg \| sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg`<br>`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release \|\| lsb_release -cs) main" \| sudo tee /etc/apt/sources.list.d/hashicorp.list`<br>`sudo apt update && sudo apt install terraform` |
| **CentOS/RHEL** | Package Manager | `sudo yum install -y yum-utils`<br>`sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo`<br>`sudo yum -y install terraform` |
| **Amazon Linux** | Package Manager | `sudo yum install -y yum-utils shadow-utils`<br>`sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo`<br>`sudo yum -y install terraform` |
| **macOS** | Homebrew | `brew tap hashicorp/tap`<br>`brew install hashicorp/tap/terraform` |
| **macOS (Intel)** | Manual | `curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_amd64.zip`<br>`unzip terraform_1.13.0_darwin_amd64.zip`<br>`sudo mv terraform /usr/local/bin/` |
| **macOS (Apple Silicon)** | Manual | `curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_arm64.zip`<br>`unzip terraform_1.13.0_darwin_arm64.zip`<br>`sudo mv terraform /usr/local/bin/` |
| **All Platforms** | Autocomplete | `terraform -install-autocomplete` |

### Core Commands

| Command | Description |
|---------|-------------|
| `terraform version` | Get the terraform version |
| `terraform init` | Initialize configuration (generate .terraform & .terraform.lock.hcl) |
| `terraform init -backend-config="key=value"` | Initialize with backend configuration |
| `terraform init -backend-config="backend.conf"` | Initialize with backend configuration file |
| `terraform init -backend=false` | Skip backend initialization entirely |
| `terraform init -upgrade` | Initialize and upgrade providers |
| `terraform init -upgrade=hashicorp/aws` | Upgrade only specific providers |
| `terraform init -get-plugins=false` | Initialize without plugins (offline mode) |
| `terraform init -get=false` | Skip getting/updating modules |
| `terraform init -reconfigure` | Reconfigure backend ignoring saved configuration |
| `terraform init -migrate-state` | Migrate state from another backend |
| `terraform init -input=false` | Skip interactive prompts |
| `terraform init -verify-plugins=false` | Skip plugin signature verification |
| `terraform init -lock=false` | Don't hold state lock during migration (dangerous) |
| `terraform init -lock-timeout=120s` | Change state lock timeout |

### Authentication

| Command | Description |
|---------|-------------|
| `terraform login` | Log in to Terraform Cloud |
| `terraform logout` | Log out from Terraform Cloud |
| `terraform login <hostname>` | Log in to a different host |

### Module Management

| Command | Description |
|---------|-------------|
| `terraform get` | Get modules |
| `terraform get -update` | Update modules |
| `terraform graph -type=plan \| grep module` | Show module tree |

### Planning & Validation

| Command | Description |
|---------|-------------|
| `terraform plan` | Create an execution plan |
| `terraform plan -out=tfplan` | Save plan to a file |
| `terraform plan -var-file="prod.tfvars"` | Plan with variable files |
| `terraform plan -var="region=us-west-2"` | Plan with inline variables |
| `terraform plan -replace aws_instance.example` | Replace selected resources |
| `terraform plan -target=module.vpc` | Target specific resources |
| `terraform validate` | Validate configuration files |
| `terraform fmt` | Format configuration files |
| `terraform fmt -check` | Check if input is formatted |
| `terraform fmt -diff` | Display formatting differences |
| `terraform fmt -recursive` | Format files in subdirectories |

### Apply & Deploy

| Command | Description |
|---------|-------------|
| `terraform apply` | Apply changes |
| `terraform apply tfplan` | Apply a saved plan |
| `terraform apply -auto-approve` | Auto-approve without confirmation |
| `terraform apply -target=aws_instance.example` | Apply with specific targets |
| `terraform apply -var="key=value"` | Apply with specific variable |
| `terraform apply -var-file="prod.tfvars"` | Apply with variable files |
| `terraform apply --parallelism=5` | Set number of simultaneous operations |

### Destroy & Cleanup

| Command | Description |
|---------|-------------|
| `terraform destroy` | Destroy all resources |
| `terraform destroy -auto-approve` | Destroy with auto-approve |
| `terraform destroy -target=aws_instance.example` | Destroy specific resources |
| `terraform plan -destroy` | Plan destruction |
| `terraform plan -destroy -out=destroy.tfplan` | Save destroy plan to file |
| `terraform apply destroy.tfplan` | Apply destroy plan |

### State Management

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

### Workspace Management

| Command | Description |
|---------|-------------|
| `terraform workspace list` | List workspaces |
| `terraform workspace new dev` | Create new workspace |
| `terraform workspace select prod` | Select workspace |
| `terraform workspace show` | Show current workspace |
| `terraform workspace delete dev` | Delete workspace |

### Output & Variables

| Command | Description |
|---------|-------------|
| `terraform output` | Show outputs |
| `terraform output instance_ip` | Show specific output |
| `terraform output -json` | Show outputs in JSON format |
| `terraform output -raw <name>` | Show specific output value without quotes |

### Debugging & Troubleshooting

| Command | Description |
|---------|-------------|
| `export TF_LOG=DEBUG; terraform plan` | Enable debug logging |
| `TF_LOG=DEBUG terraform init` | Enable debug logging for specific command |
| `export TF_LOG_PATH=./terraform.log; terraform apply` | Log to file |
| `unset TF_LOG` or `export TF_LOG=` | Disable logging |

**Available Log Levels:** TRACE, DEBUG, INFO, WARN, ERROR (in order of decreasing verbosity)

### Terraform Console Functions

| Function | Example |
|----------|---------|
| `upper()` | `echo 'upper("hello")' \| terraform console` |
| `join()` | `echo 'join(",",[\"a\",\"b\",\"c\"])' \| terraform console` |
| `split()` | `echo 'split(",","a,b,c")' \| terraform console` |
| `replace()` | `echo 'replace("hello world", "world", "terraform")' \| terraform console` |
| `length()` | `echo 'length([\"a\",\"b\",\"c\"])' \| terraform console` |
| `element()` | `echo 'element([\"a\",\"b\",\"c\"], 1)' \| terraform console` |
| `contains()` | `echo 'contains([\"a\",\"b\",\"c\"], \"b\")' \| terraform console` |
| `base64encode()` | `echo 'base64encode("hello")' \| terraform console` |
| `base64decode()` | `echo 'base64decode("aGVsbG8=")' \| terraform console` |
| `jsonencode()` | `echo 'jsonencode({name="test"})' \| terraform console` |

### Graph & Dependencies

| Command | Description |
|---------|-------------|
| `terraform graph` | Generate dependency graph |
| `terraform graph > graph.dot` | Save graph to file |
| `terraform graph \| dot -Tpng > graph.png` | Convert to PNG (requires Graphviz) |
| `terraform graph -draw-cycles` | Show resource dependencies |

### Provider Management

| Command | Description |
|---------|-------------|
| `terraform providers` | Show providers |
| `terraform providers lock` | Lock provider versions |
| `terraform providers lock -platform=linux_amd64 -platform=darwin_amd64 -platform=windows_amd64` | Pre-populate hashes for multiple platforms |
| `terraform providers mirror /path/to/mirror` | Mirror providers for offline use |

### State File Operations

| Command | Description |
|---------|-------------|
| `cp terraform.tfstate terraform.tfstate.backup` | Backup state file |
| `cp terraform.tfstate.backup terraform.tfstate` | Restore state file |
| `terraform show -json > state.json` | Convert state to JSON |
| `diff terraform.tfstate.backup terraform.tfstate` | Compare state files |
| `terraform plan -refresh-only` | Inspect resource drift without updating state |
| `terraform apply -refresh-only` | Plan and refresh the state file |

### Testing & Validation

| Command | Description |
|---------|-------------|
| `terraform validate` | Validate syntax (requires terraform init) |
| `terraform validate -backend=false` | Validate code, skip backend validation |
| `terraform validate -json` | Output in machine-readable JSON format |

### Utilities

#### tfenv

```bash
# Install tfenv on macOS
brew install tfenv

# Install tfenv on Linux
git clone --depth=1 https://github.com/tfutils/tfenv.git ~/.tfenv
echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bash_profile

# List available versions
tfenv list-remote

# Install specific version
tfenv install 1.5.0

# Use specific version
tfenv use 1.5.0

# Install latest version
tfenv install latest

# Show current version
tfenv version
```

#### tflint

```bash
# Install tflint on macOS
brew install tflint

# Install tflint on Linux
curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash

# Lint with TFLint
tflint
```

#### tfsec

```bash
# Install tfsec on macOS
brew install tfsec

# Install tfsec on Linux
curl -s https://raw.githubusercontent.com/aquasecurity/tfsec/master/scripts/install_linux.sh | bash

# Scan for security issues (using tfsec)
tfsec .

# Scan specific directory
tfsec ./infrastructure

# Output JSON format
tfsec --format json .
```

#### terraform-docs

```bash
# Install terraform-docs on macOS
brew install terraform-docs

# Install terraform-docs on Linux
curl -sSLo ./terraform-docs.tar.gz https://terraform-docs.io/dl/v0.20.0/terraform-docs-v0.20.0-$(uname)-amd64.tar.gz
tar -xzf terraform-docs.tar.gz
chmod +x terraform-docs
mv terraform-docs /some-dir-in-your-PATH/terraform-docs

# Generate documentation (using terraform-docs)
terraform-docs markdown . > README.md
```

### Terraform Variables

```bash
export TF_VAR_region="us-west-2"
export TF_VAR_instance_type="t3.micro"
export TF_LOG=DEBUG
export TF_LOG_PATH="./terraform.log"
export TF_CLI_CONFIG_FILE="$HOME/.terraformrc"
```

### State Management Best Practices

| Practice | Command | Description |
|----------|---------|-------------|
| **Backup** | `cp terraform.tfstate terraform.tfstate.backup.$(date +%Y%m%d_%H%M%S)` | Backup state before major operations |
| **Remote State** | `terraform init -backend-config="bucket=my-tf-state" -backend-config="key=prod/terraform.tfstate"` | Use remote state |
| **State Locking** | `terraform init -backend-config="dynamodb_table=terraform-locks"` | Enable state locking |

### Useful Aliases

Add these to your `~/.zshrc` or `~/.bashrc`:

```bash
alias tf="terraform"
alias tfi="terraform init"
alias tfp="terraform plan"
alias tfa="terraform apply"
alias tfd="terraform destroy"
alias tfs="terraform show"
alias tfo="terraform output"
alias tfv="terraform validate"
alias tff="terraform fmt"
alias tfw="terraform workspace"
alias tfws="terraform workspace show"
alias tfsl="terraform state list"
```

### Useful Functions

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

### Best Practices & Tips

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
