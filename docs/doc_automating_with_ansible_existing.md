# Setting up an existing Ansible environment

1. Clone or download the Github repository.

2. Update your existing inventory.
   -  In the `lmt_server` parameter, define connection settings to the host where the License Metric Tool server is installed.
   - Define setting of License Metric Tool (parameters that start with `lmt_`) to match your environment. For the list of parameters, see: [License Metric Tool parameters](doc_lmt_parameters.md).
        >**Note:** You do not need to define the settings if you use the default values.

3. Schedule the `lmt_disconnected_scans_collector.yml` playbook to run every day. The playbook is configured to run on all computers as it targets the `all` Ansible group. To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on `unix1` host and `windows` group of hosts run the following command.

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml -l lmt_server,localhost,unix1,windows`
