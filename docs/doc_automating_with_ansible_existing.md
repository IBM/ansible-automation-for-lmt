# Setting up an existing Ansible environment

1. Clone or download the Github repository.

2. Update your existing inventory.
   - Mark the managed node on which the License Metric Tool server is installed with the `lmt_server` tag.
   - Define setting of License Metric Tool (parameters that start with `lmt_`) to match your environment. For the list of parameters, see: [License Metric Tool parameters](doc_lmt_parameters.md).
        >**Note:** You do not need to define the settings if you use the default values.
