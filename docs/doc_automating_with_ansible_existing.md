# Setting up an existing Ansible environment
If Ansible is implemented in your organization, clone or download this Github repository. Then, update your existing inventory.

<br>

## Procedure

1. Clone or download the Github repository.

2. Update your existing inventory.
   - Mark the managed node on which the License Metric Tool server is installed with the `lmt_server` tag.
   - Define setting of License Metric Tool (parameters that start with `lmt_`) to match your environment. For the list of parameters, see: [License Metric Tool parameters](doc_lmt_parameters.md).
        >**Note:** You do not need to define the settings if you use the default values.

<br>
     
## What to do next

Run the `lmt_install_or_upgrade_scanner.yml` playbook to install the disconnected scanner and perform its initial configuration. For more information, see: [Install the disconnected scanner and schedule scans](doc_install_scanner.md).