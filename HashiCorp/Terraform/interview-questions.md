# Terraform Interview Questions

**1. When using Terraform to create an RDS, how do you save the DB username and
password securely?**

- **Answer:** Utilize a secrets management tool like HashiCorp Vault. Vault is
  designed explicitly to store sensitive values such as database credentials.
  Terraform can interact with Vault to retrieve these secrets at runtime,
  keeping them out of your configuration files.

**2. Your security team has insisted that certain security-related packages must
be present on all instances within a defense project. How would you design a
solution to ensure these packages are present on all relevant EC2 instances?**

- **Answer:** Employ Packer to create custom AMIs (Amazon Machine Images) that
  pre-install the required security packages.

  - Packer allows you to define the operating system, packages, and any
    additional configuration needed for your EC2 instances.
  - When launching new EC2 instances, use the custom AMI created by Packer to
    ensure they automatically include the necessary security packages.

**3. What is remote execution in Terraform, and when would you use it?**

- **Answer:** Remote execution in Terraform lets you run commands or scripts on
  remote resources after they have been provisioned.

  - **Use Cases:**
    - **Configuration Management:** Execute configuration management tools like
      Ansible or Chef on newly created instances to install software or apply
      settings.
    - **Post-provisioning Tasks:** Perform actions like joining instances to a
      domain or registering them with a monitoring system.

**4. How do you validate variables during Terraform plan time? For example,
ensure an AMI variable conforms to a specific format.**

- **Answer:** Employ input variable validation within your Terraform
  configuration.

  - **Example (Enforcing AMI format):**
    ```terraform
    variable "ami" {
      type = string
      validation {
        condition     = can(regex("^ami-[0-9a-f]{8}$", var.ami))
        error_message = "AMI must match the format 'ami-xxxxxxxx' (where 'x' is a hex digit)."
      }
    }
    ```

**5. What does Terraform state locking mean, and what are its practical uses?**

- **Answer:** State locking prevents simultaneous changes to your infrastructure
  by multiple users or processes. When one Terraform operation is in progress,
  it acquires a lock on the state file, preventing conflicts and potential
  corruption.

  - **Practical Use:** Essential in collaborative environments where multiple
    team members work with the same Terraform configuration. State locking helps
    maintain consistency and reduces the risk of unexpected infrastructure
    modifications.

**6. When running `terraform apply`, the process hangs. Canceling might corrupt
the Terraform state. How do you debug this?**

- **Answer:**
  1. **Examine Logs:** Look for error messages, warnings, or resource timeouts.
  2. **Check Resource Dependencies:** Ensure correct 'depends_on' relationships
     prevent deadlock conditions.
  3. **Inspect Provider Issues:** Review the provider's documentation to see if
     there are known bugs or limitations relating to the resource you're trying
     to modify.
  4. **Retrieve a Fresh State:** Use `terraform state pull` to obtain the latest
     state file from the configured backend (if you have one). This ensures your
     local state is synchronized.
  5. **Re-Attempt Application:** Execute `terraform apply` again. With a
     refreshed state, the process may be able to proceed successfully.
