
# Managing disconnected scans with Ansible

## Disconnected scans

Disconnected scans allow for discovering software and hardware inventory on computers that do not have connection to the BigFix server. Scripts that are provided in the disconnected scanner package initiate software and capacity scans, and create a package with scan results that you later upload to License Metric Tool.
You can use Ansible to automate the process of uploading scan results from the computers to the License Metric Tool server.

>**Note:** For more information, see: [Disconnected scan configuration](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/c_disc_main.html) in the License Metric Tool documentation.

## Automating collection of disconnected scan results with Ansible 

[Ansible](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. To automate the management of the disconnected scans with Ansible, perform the following steps.


1. Define the disconnected data source.

2. Install and set up disconnected scans.

3. Configure Ansible to manage the selected computers.

For detailed instructions, see: [Automating collection of disconnected scan results with Ansible](docs/doc_automating_with_ansible.md).

## Managing the disconnected scanner with AWX or Tower

AWX is an open source, easy-to-use user interface, dashboard and REST API for Ansible. Ansible Tower is a commercial version of AWX supported by Red Hat.
You can use either of these tools to manage the disconnected scanner. For detailed instructions, see:  [Managing the disconnected scanner with AWX or Tower](docs/doc_automating_with_awx_tower.md).
 
