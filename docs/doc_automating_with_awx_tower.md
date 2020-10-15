
## Managing the disconnected scanner with AWX or Tower

[AWX]((https://github.com/ansible/awx)) is an open source, easy-to-use UI, dashboard and REST API for Ansible. [Ansible Tower](https://www.ansible.com/products/tower) is a commercial version of AWX supported by Red Hat. To manage the disconnected scanner with either of these tools, perform the following steps.

1. Install AWX or Tower.

2. Create a New Project. Specify the SCM TYPE as Git and provide a URL to the following repository:  [https://github.com/IBM/ansible-automation-for-lmt](https://github.com/IBM/ansible-automation-for-lmt).

3. Create a New Inventory. 

   - In the `lmt_server` parameter, define connection settings to the host where the License Metric Tool server is installed.
 
   - Define setting of License Metric Tool (parameters that start with `lmt_`) to match your environment. For the list of parameters, see: [License Metric Tool parameters](doc_lmt_parameters.md).
    >**Note:** You do not need to define the settings if you use the default values.

4. Choose Credential type and define it.

5. Create a Job Template. Provide its name, inventory, credentials, and project. Then, choose `lmt_disconnected_scans_collector.yml` as the playbook.

6. Schedule the job to run every day.
