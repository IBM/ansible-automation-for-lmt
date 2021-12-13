
## Managing disconnected scans with AWX or Tower

[AWX]((https://github.com/ansible/awx)) is an open source, easy-to-use UI, dashboard and REST API for Ansible. [Ansible Tower](https://www.ansible.com/products/tower) is a commercial version of AWX supported by Red Hat. To manage the disconnected scans with either of these tools, perform the following steps.

1. Install AWX or Tower.

2. Create a New Project. Specify the SCM TYPE as Git and provide a URL to the following repository:  [https://github.com/IBM/ansible-automation-for-lmt](https://github.com/IBM/ansible-automation-for-lmt).

3. Create a New Inventory. 

   - In the `lmt_server` parameter, define connection settings to the host where the License Metric Tool server is installed.
 
   - Define setting of License Metric Tool (parameters that start with `lmt_`) to match your environment. For the list of parameters, see: [Parameters used in playbooks](doc_lmt_parameters.md).
    >**Note:** You do not need to define the settings if you use the default values.

5. Create a Job Template to install the disconnected scanner. Provide its name, inventory, credentials, and project. Then, choose `lmt_install_or_upgrade_scanner` as the playbook. 

6. Create a Job Template to schedule daily collection of scan results. Provide its name, inventory, credentials, and project. Then, choose `lmt_collect_results` as the playbook. 