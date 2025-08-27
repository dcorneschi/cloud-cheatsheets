# Terraform Cheatsheet

## Terraform Installation Guide

### Linux

#### Using Package Manager (Ubuntu/Debian)

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

#### Using Package Manager (CentOS/RHEL)

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
```

#### Using Package Manager (Amazon Linux)

```bash
sudo yum install -y yum-utils shadow-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform
```

### macOS

#### Using Homebrew

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

#### Manual Installation (Intel Macs)

```bash
curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_amd64.zip
unzip terraform_1.13.0_darwin_amd64.zip
sudo mv terraform /usr/local/bin/
```

#### For Apple Silicon Macs

```bash
curl -O https://releases.hashicorp.com/terraform/1.13.0/terraform_1.13.0_darwin_arm64.zip
unzip terraform_1.13.0_darwin_arm64.zip
sudo mv terraform /usr/local/bin/
```

### Install the autocomplete package (add the line ~/.bashrc)

```bash
terraform -install-autocomplete
```

### Useful Links

- [Official Terraform Downloads](https://www.terraform.io/downloads)
- [HashiCorp Releases](https://releases.hashicorp.com/terraform/)

## Initialization & Setup

```bash
# Get the terraform version
terraform version

# Initialize a new or existing Terraform configuration (generate .terraform & .terraform.lock.hcl)
terraform init

# Initialize with backend configuration
terraform init -backend-config="key=value"

# Initialize with backend configuration file
terraform init -backend-config="backend.conf"

# Skip backend initialization entirely
terraform init -backend=false

# Initialize and upgrade providers
terraform init -upgrade

# Upgrade only specific providers
terraform init -upgrade=hashicorp/aws

# Initialize without plugins (offline mode)
terraform init -get-plugins=false

# Skip getting/updating modules
terraform init -get=false

# Reconfigure backend ignoring any saved configuration
terraform init -reconfigure

# Migrate state from another backend
terraform init -migrate-state

# Skip interactive prompts and use default values
terraform init -input=false

# Skip plugin signature verification
terraform init -verify-plugins=false

# Don't hold a state lock during backend migration (dangerous)
terraform init -lock=false

# Change state lock timeout (default is zero seconds)
terraform init -lock-timeout=120s
```

## Terraform Cloud / Remote Authentication

```bash
# Interact with terraform cloud
terraform [login|logout]

# Log in to a different host
terraform login <hostname>
```

## Module Management

```bash
# Get modules
terraform get

# Update modules
terraform get -update

# Show module tree
terraform  -type=plan | grep module
```

### Planning

```bash
# Create an execution plan
terraform plan

# Save plan to a file
terraform plan -out=tfplan

# Plan with variable files
terraform plan -var-file="prod.tfvars"

# Plan with inline variables
terraform plan -var="region=us-west-2"

# Replacing selected resources (don't use terraform taint/untaint anymore)
terraform plan -replace aws_instance.example
terraform apply -replace aws_instance.example

# Target specific resources to reduce planning time
terraform plan -target=module.vpc
```

### Validation

```bash
# Validate configuration files
terraform validate

# Format configuration files
terraform fmt

# Check if the input is formatted
terraform fmt -check

# Display differences between original files and formatting change
terraform fmt -diff                      

# Format files in subdirectories as well
terraform fmt -recursive
```

### Apply & Deploy

```bash
# Apply changes
terraform apply

# Apply a saved plan
terraform apply tfplan

# Auto-approve without confirmation
terraform apply -auto-approve

# Apply with specific targets
terraform apply -target=aws_instance.example

# Apply the changes with a specific variable
terraform apply -var="key=value"     

# Apply with variable files
terraform apply -var-file="prod.tfvars"

#Number of simultaneous resource operations
terraform apply --parallelism=5
```

### Destroy & Cleanup

```bash
# Destroy all resources
terraform destroy

# Destroy with auto-approve
terraform destroy -auto-approve

# Destroy specific resources
terraform destroy -target=aws_instance.example

# Plan destruction
terraform plan -destroy

# Save the destroy plan in a file and apply it (no confirmation for apply)
terraform plan -destroy -out=destroy.tfplan
terraform apply destroy.tfplan
```

### State Management

```bash
# Show current state
terraform show

# Inspect the plan
terraform show my.plan

# List resources in state
terraform state list

# Count resources
terraform state list | wc -l

# Count resources by type
terraform state list | cut -d. -f1 | sort | uniq -c

# Get all resource dependencies
terraform show -json | jq '.values.root_module.resources[] | {address: .address, depends_on: .depends_on}'

# Show specific resource
terraform state show aws_instance.example

# Remove resource from state
terraform state rm aws_instance.example

# Import existing resource
terraform import aws_instance.example i-1234567890abcdef0

# Move resource in state
terraform state mv aws_instance.example aws_instance.new_name

# Pull remote state
terraform state pull
terraform state pull > terraform.tfstate

# Push local state to remote (if auth token expires)
terraform state push terraform.tfstate

# Manually unlocking state configuration
terraform force-unlock -force my_lock_id       
```

### Workspace Management

```bash
# List workspaces
terraform workspace list

# Create new workspace
terraform workspace new dev

# Select workspace
terraform workspace select prod

# Show current workspace
terraform workspace show

# Delete workspace
terraform workspace delete dev
```

### Output & Variables

```bash
# Show outputs
terraform output

# Show specific output
terraform output instance_ip

# Show outputs in JSON format
terraform output -json

 #Show a specific output value without quotes
terraform output -raw <name>
```

### Debugging & Troubleshooting

You can set TF_LOG to one of the log levels (in order of decreasing verbosity) TRACE, DEBUG, INFO, WARN or ERROR to change the verbosity of the logs.

```bash
# Enable debug logging
export TF_LOG=DEBUG
terraform plan

# Enable debug logging for a specific command
TF_LOG=DEBUG terraform init

# Log to file
export TF_LOG_PATH=./terraform.log
terraform apply

# Disable logging
unset TF_LOG
export TF_LOG=
```

### Terraform Console

```bash
# Basic console usage
echo 'expression' | terraform console

# String functions
echo 'upper("hello")' | terraform console
echo 'join(",",["a","b","c"])' | terraform console
echo 'split(",","a,b,c")' | terraform console
echo 'replace("hello world", "world", "terraform")' | terraform console

# List functions
echo 'length(["a","b","c"])' | terraform console
echo 'element(["a","b","c"], 1)' | terraform console
echo 'contains(["a","b","c"], "b")' | terraform console

# Encoding functions
echo 'base64encode("hello")' | terraform console
echo 'base64decode("aGVsbG8=")' | terraform console
echo 'jsonencode({name="test"})' | terraform console
```

### Graph & Dependencies

```bash
# Generate dependency graph
terraform graph

# Generate graph and save to file
terraform graph > graph.dot

# Convert to PNG (requires Graphviz)
terraform graph | dot -Tpng > graph.png

# Show resource dependencies
terraform graph -draw-cycles
```

### Provider Management

```bash
# Show providers
terraform providers

# Lock provider versions
terraform providers lock

# Pre-populate hashes for multiple platforms
terraform providers lock -platform=linux_amd64 -platform=darwin_amd64 -platform=windows_amd64

# Mirror providers for offline use
terraform providers mirror /path/to/mirror
```

### State File Operations

```bash
# Backup state file
cp terraform.tfstate terraform.tfstate.backup

# Restore state file
cp terraform.tfstate.backup terraform.tfstate

# Convert state to JSON
terraform show -json > state.json

# Compare state files
diff terraform.tfstate.backup terraform.tfstate

# Inspect resource drift without updating the state file
terraform plan -refresh-only

# Plan and refresh the state file
terraform apply -refresh-only
```

### Testing & Validation

```bash
# Validate syntax
terraform validate (requires "terraform init")

#Validate code skip backend validation
terraform validate -backend=false validate

# Output in a machine-readable JSON format
terraform validate -json
```

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

```bash
# Always backup state before major operations
cp terraform.tfstate terraform.tfstate.backup.$(date +%Y%m%d_%H%M%S)

# Use remote state
terraform init -backend-config="bucket=my-tf-state" -backend-config="key=prod/terraform.tfstate"

# Enable state locking
terraform init -backend-config="dynamodb_table=terraform-locks"
```

## Useful Aliases

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

## Useful functions 

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

## Best Practices & tips

1. Always commit lock files to version control alongside your Terraform configuration files.
2. Never add `.terraform.lock.hcl to` `.gitignore` - ignoring lock files defeats their purpose.
3. Never commit .tfvars files with secrets.
3. Store state files remotely and securely.
5. Use workspaces for environment separation.
6. Use modules for reusable components.
7. Pin provider versions in production.
8. Always run `terraform plan` before `apply`.
9. Review and understand the execution plan before applying.

---

<div align="center">

**Made with ❤️ by [Daniel Corneschi](https://github.com/dcorneschi)**

If you enjoyed this project, please consider giving it a ⭐️!

</div>
