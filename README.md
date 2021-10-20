# Managing License Metric Tool disconnected scan with Ansible

---
<div style="text-align: center;"><h4 style="background-color: #fef6c8;">Important</h4></div>

The minimal supported version of License Metric Tool disconnected scanner to use with Ansible automation is **9.2.22**.<br>
The minimal supported version of Ansible is **2.10.2**. However, it is recommended to use the latest version of Ansible that is available.

---
<div style="text-align: center;"><h4 style="background-color: #fef6c8;">Version of License Metric Tool playbooks</h4></div>

The current version of License Metric Tool playbooks is **9.2.25**.

---
<div style="text-align: center;"><h4 style="background-color: #fef6c8;">Supported operating systems</h4></div>

Playbooks that are delivered with License Metric Tool are supported on AIX, Linux, Solaris and Windows. They are not supported on HP-UX and IBM i. For information about exact versions, see the [list of operating systems that are supported by the disconnected scanner](https://www.ibm.com/support/pages/node/561443). 

---

## Disconnected scan

Disconnected scans allow for discovering software and hardware inventory on computers that do not have connection to the BigFix server. Scripts that are provided in the disconnected scanner package initiate software and capacity scans, and create a package with scan results that you later upload to License Metric Tool.
You can use Ansible to automate the process of uploading scan results from the managed nodes to the License Metric Tool server.

>**Note:** For more information, see: [Disconnected scan configuration](http://ibm.biz/disconnected_scan_config) in the License Metric Tool documentation.

## Automating collection of disconnected scan results with Ansible 

[Ansible](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. To automate the management of disconnected scans with Ansible, perform the following steps.


1. Define the disconnected data source.

2. Configure Ansible to manage the selected computers.

3. Install the disconnected scanner and schedule scans.

4. Schedule daily collection of scan results from the managed nodes.

5. Maintain the environment. Maintenance includes actions such as configuration changes, scanner upgrades, troubleshooting of potential issue.

For detailed instructions, see: [Automating collection of disconnected scan results with Ansible](docs/doc_automating_with_ansible.md).

## Inventory of supported Ansible playbooks for disconnected scan management 

This repository contains the following set of Ansible playbooks that can help you with performing basic configuration and management of the disconnected scanners. 
- `lmt_collect_results.yml` - collects scan results from managed nodes and uploads them to the disconnected data source directory on the License Metric Tool server.
- `lmt_install_or_upgrade_scanner.yml` - installs or upgrades the disconnected scanner to the same version as the License Metric Tool server.
- `lmt_collect_status.yml` - collects basic information such as the version of the installed scanner and its status.
- `lmt_collect_troubleshooting_data.yml` - collects troubleshooting data such as logs and raw scan results from scanners. The data can be useful for investigating potential issues.
- `lmt_reconfigure_scanner.yml` - updates the scanner configuration, for example, changes frequency of software scans.
- `lmt_uninstall_scanner.yml` - uninstalls the disconnected scanner from the targeted computers.

For more information about playbooks, see the following links:
- [License Metric Tool playbooks](docs/doc_playbooks_list.md)
- [License Metric Tool parameters](docs/doc_lmt_parameters.md)
- [Updating License Metric Tool playbooks](docs/doc_updating_lmt_playbooks.md) 

An inventory template that you can use with License Metric Tool playbooks is in the `lmt_inventory_template.yml` file. 

## Managing the disconnected scanner with AWX or Tower

AWX is an open source, easy-to-use user interface, dashboard and REST API for Ansible. Ansible Tower is a commercial version of AWX supported by Red Hat.
You can use either of these tools to manage the disconnected scanner. For detailed instructions, see:  [Managing the disconnected scanner with AWX or Tower](docs/doc_automating_with_awx_tower.md).

## Defect fixes
 
 For information about defects that were solved in Ansible playbooks that are delivered with License Metric Tool, see: [Release notes](docs/release_notes.md).


