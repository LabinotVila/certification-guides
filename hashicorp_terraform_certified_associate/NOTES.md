Notes regarding `HashiCorp Certified: Terraform Associate (003)` exam.

<small>


1. You want to restrict your team members to specific modules that are approved by the organization’s security team when using Terraform Cloud. What feature you should use?
    1. Terraform Workspaces → commands: tf workspace show / list / select {name} / new {name} / delete {name}
    2. Terraform Cloud Organizations → are a shared space for one or more teams to collaborate on workspaces.
    3. Terraform Registry (public) → where modules and providers are stored.
    4. <span style="background-color:#70c286;">Terraform Registry (private) → many organizations use modules, providers, or Sentinel policies that cannot or do not need to be publicly available.</span>

2. You have a configuration file that you've deployed to one AWS region already but you want to deploy the same configuration file to a second AWS region without making changes to the configuration file. What feature of Terraform can you use to accomplish this?
    1. <span style="background-color:#70c286">terraform workspace</span>
    2. terraform import → applies to your state configuration the current state of cloud; terraform apply -refresh does an apply, but by making sure that infrastructure is up to date, then continues procedures as normally.
    3. terraform get → this loops through all modules, and upgrades the version (if possible, by checking provider configuration).

3. True or False? In order to use the `terraform console` command, the CLI must be able to lock state to prevent changes.
    1. <span style="background-color:#70c286">true</span> - state lock is acquired when the command is actually used: `$ terraform console` → `Acquiring state lock. This may take a few moments…`

4. After deploying a new virtual machine using Terraform, you find that the local script didn't run properly. However, Terraform reports the virtual machine was successfully created. How can you force Terraform to replace the virtual machine without impacting the rest of the managed infrastructure?
    1. <span style="background-color:#70c286">terraform apply -repalce</span> - tag the resource for replacement, meaning that next apply, only this resource will be affected

5. Which statement is true regarding using Sentinel in Terraform?
    1. <span style="background-color:#70c286">Sentinel runs before a configuration is applied, therefore potentially reducing cost for public cloud resources</span> → it runs before `apply` and after `plan`. It is written in `Sentinel` language (which is unique). You can potentially reduce costs on public cloud resources by NOT deploying resources that do NOT conform to policies enforced by Sentinel.

6. What log should you set for the `TF_LOG` environment variable, if you want to have the most verbose logs?
    1. <span style="background-color:#70c286">TRACE</span> - is the most verbose logging; env variable → `TF_LOG`.  Other types: `DEBUG`, `INFO`, `WARN`, `ERROR`. If we want to use logging separately, we can use `TF_LOG_CORE` and `TF_LOG_PROVIDE`R. To persist logging output, we can set `TF_LOG_PATH` (always appended to the file). This requires `TF_LOG` to be set too.

7. You are using Terraform to manage some of your AWS infrastructure. You notice that a new version of the provider now includes additional functionality you want to take advantage of. What command do you need to run to upgrade the provider?
    1. <span style="background-color:#70c286">terraform init -upgrade</span>

8. Environment variables can be used to set the value of input variables. The environment variables must be in the format:
    1. <span style="background-color:#70c286">TF_VAR_variable_name</span> → for example: `TF_VAR_environment=dev` - this will be the last checked value.

9. A "backend" in Terraform determines how state is loaded and how an operation such as `apply`  is executed. Which of the following is not a supported backend type?
    1. <span style="background-color:#70c286">github</span> - supported backends: 
        1. `local` → local PC storage, somewhere in your file system
        2. `remote` → somewhere in Terraform Cloud organization.
        3. `azurerm` → azure blob storage
        4. `s3` → amazon s3
        5. `gcs` → google cloud storage
        6. `consul` → consul is a service networking platform developed by HashiCorp.
        7. `cos` → terraform object cloud storage
        8. `http` → store the state in a simple REST client. State will be fetched by `GET`, updated by `POST` and deleted by `DELETE`.
        9. `K8`
        10. `oss` → Alibaba cloud
        11. `pg` → Postgres database

10. Which of the following is a valid variable name in Terraform?
    1. <span style="background-color:#70c286">invalid</span> → reserved variable names in Terraform:
        1. `terraform`, `variable`, `locals`, `output`, `module`, `resource`, `data`, `provider`
        2. `lifecycle`, `prevent_destroy`, `ignore_changes`, `provisioners`, `connection`
        3. `count`, `for_each`, `depends_on`, `provider`
        4. `length`, `lookup`, `element`, `locals`, `true`, `false`, `null`, `for`, `if`

11. Terraform Cloud is more powerful when you integrate it with your version control system (VCS) provider. Select all the non-supported VCS providers from the answers below.
    1. <span style="background-color:#70c286">CVS Version Control</span> → supported version controls of Terraform:
        1. `Github` → App for TFE, OAuth, Enterprise, com
        2. `Gitlab` → EE and CE, com
        3. `Bitbucket` → Cloud, Data Center
        4. `Azure` → DevOps Servers, Services

12. What is the purpose of using the `local-exec` provisioner and `remote-exec`?
    1. The `local-exec` provisioner is used to execute a command on the machine running Terraform, rather than on a remote resource
    2. The `remote-exec` provisioner invokes a script on a remote resource after it is created → supports `ssh` and `winrm`
        1. `ssh` → secure shell: method for securely sending commands to a computer over an unsecured network
        2. `winrm` → windows remote management: execute commands, run scripts for local or remote machines

13. What are the core Terraform workflow steps?
    1. <span style="background-color:#70c286">Write, Plan and Apply</span>

14. In the `terraform` block, which configuration would be used to identify the specific version of a provider required?
    1. <span style="background-color:#70c286">required_providers</span>

15. Which of the following Terraform files should be ignored by Git when committing code to a repo?
    1. <span style="background-color:#70c286">terraform.tfvars</span> → likely contains sensitive data, such as passwords, etc.
    2. <span style="background-color:#70c286">terraform.tfstate</span> → as a rule of thumb, state should be stored in a backend, and not in Git

16. What is the difference between `terraform show` and `terraform state list`?
    1. <span style="background-color:#70c286">Below</span>:
        
        ```scala
        provider "aws" {
          region = "us-west-2"
        }
        
        resource "aws_instance" "web_server" {
          ami           = "ami-12345678"
          instance_type = "t2.micro"
        }
        
        resource "aws_instance" "db_server" {
          ami           = "ami-87654321"
          instance_type = "t2.micro"
        }
        
        // terraform apply -> terraform show
        // aws_instance.web_server:
        resource "aws_instance" "web_server" {
          ami                          = "ami-12345678"
          instance_type                = "t2.micro"
          ...
        }
        
        // aws_instance.db_server:
        resource "aws_instance" "db_server" {
          ami                          = "ami-87654321"
          instance_type                = "t2.micro"
          ...
        }
        
        // terraform state list
        aws_instance.web_server
        aws_instance.db_server
        ```

17. Do workspaces provide similar functionality in the open-source and Terraform Cloud versions of Terraform?
    1. <span style="background-color:#70c286">False</span> → in OSS, are alternate state files (more like branches), in Cloud, workspaces act more like separate working directories

18. After executing a `terraform plan`, you notice a resource has a tilde (`~`) next to it. What does it mean?
    1. <span style="background-color:#70c286">The resource will be updated in place</span> → other supported operators:
        1. `+` → add operation to create a new resource
        2. `-` → `destroy` operator will be applied
        3. `- / +` → resource will be replaced
        4. `(known after apply)` → can not determine value until the operation is performed
        5. `(destroy)` → plans to destroy

19. Which of the following code snippets will properly configure a Terraform backend?
    1. <span style="background-color:#70c286">This</span>:
        ```scala
        terraform {
            backend "s3" {
                bucket = "..."
                key = "..."
                region = "..."
            }
            
            workspaces {
                name = "my-workspace"
            }
        }
        ```

20. Refresh Only explanation:
    1. In Terraform configuration file, create an example of a bucket, for example: `my-test-bucket`
    2. `apply` this change, so the resource is created and the state is reflected in state file
    3. Delete the resource manually
    4. You can use `-refresh-only` to map outside world to the state file, therefore:
        1. If you do `terraform plan -refresh-only`, an output will show where it tells that a resource has been deleted remotely, and the state will remove it as well
        2. if you do `terraform apply -refresh-only`, the state that contains the deleted resource remotely will be deleted lo
        3. If you do `terraform plan/apply`, the bucket will be created remotely

21. Environment Variables
    - export `TF_LOG=trace | debug | info | warn | error`   — to unset, `TF_LOG=` or `TF_LOG=off`.
    - `TF_INPUT=false` — Terraform will not prompt for input (any unspecified variable will remain as is). Good for continuous deployments.
    - `TF_VAR_name` — last checked for value.
    - `TF_CLI_ARGS` — additional arguments to command-line. It allows easier automation;
        - `TF_CLI_ARGS="-input=false" terraform apply -force` is the equivalent to manually typing: `terraform apply -input=false -force` .
    - `TF_DATA_DIR` — changes the location where Terraform keeps its per-working-directory data, such as the current backend configuration.
    - `TF_WORKSPACE` — For multi-environment deployment, in order to select a workspace, instead of doing `terraform workspace select your_workspace`, it is possible to use this environment variable. Using TF_WORKSPACE allow and override workspace selection.
    - `TF_IN_AUTOMATION` — If `TF_IN_AUTOMATION` is set to any non-empty value, Terraform adjusts its output to avoid suggesting specific commands to run next. This can make the output more consistent and less confusing in workflows where users don't directly execute Terraform commands, like in CI systems or other wrapping applications.
    - `TF_IGNORE` — If `TF_IGNORE` is set to "trace", Terraform will output debug messages to display ignored files and folders. This is useful when debugging large repositories with `.terraformignore` files.

22. Failed Provisioners and Tainted Resources
    - If a resource is created but failed while provisioning, the resource will be marked as tainted —> the resource has been physically created, but it is not safe to use. So, Terraform doesn’t try to re-construct the provisioner, instead, it deletes the whole resource and retries. (TF will only work on that tainted resource)

23. Terraform Output Style
    - `terraform show` — shows complete current state.
    - `terraform state list` — list of resources (comprehensively short)

24. Workspaces
    - Are technically equivalent to renaming your state file.

25. Variable presedence
    - `-var | -var-file` > `.auto.tfvars | .auto.tfvars.json` > `terraform.tfvars | terraform.tfvars.json` > `env variables`

26. State backups
    - Terraform creates a state backup on every `terraform state` command, this cannot be disabled (due to sensitivity of state files).

27. Naming Convention
    1. Providers
        1. Official: `hashicorp`/ `AWS|Azure|GCP|Kubernetes`
            1. AWS
            2. Azure
            3. GCP
            4. Kubernetes
        2. Others: `org/user_name`/ `module`, ex: `oracle/oci`
    2. Modules
        1. `key` / `value`, ex: `terraform-google-maps` / `project-factory` or `aztfmod` / `caf`


</small>