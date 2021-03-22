# Automating collection of disconnected scan results with Ansible 
[Ansible](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. To automate the management of the disconnected scanners with Ansible, perform the following steps.

### 1. Define a disconnected data source

For detailed instructions, see: [Adding a data source](https://ibm.biz/LMT_adding_data_source) in the License Metric Tool documentation.   

### 2. Configure Ansible to manage the selected computers

To use Ansible for automation, you need a control node where you can run the Ansible playbook. The control node communicates with the managed nodes (scanned computers) and collects results of the disconnected scans. The control node can be on the same computer as the License Metric Tool server or on a different one. 

**Requirements**

<details>
<summary>Ansible version</summary>

The solution is tested on Ansible 2.10.2. However, it is recommended to use the latest version of Ansible that is available.

</details>

<details>
<summary>Control node requirements</summary>

  - Python 2.7, or Python 3.5 or a higher 3.x version. 
  - [`pywinrm`](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#what-is-winrm) package installed to communicate with Windows servers over WinRM.
  - For a full list of control nodes requirements and the most up-to-date information, see: [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements) in the Ansible documentation.
	>**Note:** Control node is not officially supported on Windows.
</details>

<details>
<summary>Managed node requirements</summary>

  **\[UNIX/Linux\]** 
  - A way to communicate which usually is SSH. By default, this uses SFTP. If that is not available, you can switch to SCP in the `ansible.cfg`. 
  - Python 2.6 or a higher 2.x version, or Python 3.5 or a higher 3.x version.
  - The `lmt_install_or_upgrade_scanner` and `lmt_collect_troubleshooting_data` playbooks requires the `gzip` and `tar` commands to be present in the <code>$PATH</code> of the configured Ansible user.
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.

    >**Note:** If you have SELinux enabled on the License Metric Tool server (the `lmt_server` host) and the server is on a managed node, install `libselinux-python` to be able to copy the disconnected scan results. For more information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.
    
  **\[Windows\]** 
  - Ansible can generally manage Windows versions that are under current and extended support from Microsoft. Ansible can manage desktop operating systems including Windows 7, 8.1, and 10, and server operating systems including Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016, and 2019.
  - PowerShell 3.0 or higher and at least .NET 4.0 must be installed on the Windows host.
  - A WinRM listener must be created and activated.
  - The user must be a member of the local Administrators group or must be explicitly granted access.
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements on Windows](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#windows-setup) in the Ansible documentation.
</details>

<details>
<summary>License Metric Tool requirements for managed nodes</summary>

  - For the list of disconnected scanner requirements, see: [IBM License Metric Tool - Supported Operating Systems](https://ibm.biz/LMT_supported_OS). On the page that opens, click Disconnected scanner.
</details>

**Procedure**

- If you are setting up a new Ansible environment, see: [Setting up a new Ansible environment](doc_automating_with_ansible_new.md).
- If you already have Ansible set up, see: [Setting up an existing Ansible environment](doc_automating_with_ansible_existing.md).

### 3. Install the disconnected scanner and schedule scans

#### **Installation by using a playbook**

The recommended approach for installing and upgrading the disconnected scanners on your managed nodes is by using the `lmt_install_or_upgrade_scanner.yml` playbook.

**Requirements**

- Before you run this playbook, review the default configuration parameters and adjust them as needed. For more information, see: [License Metric Tool parameters](doc_lmt_parameters.md). 
- This playbook requires full root or administrator rights. Ensure that you configure valid privilege escalation in the Ansible inventory or use root or administrator accounts directly.

**Procedure**
- By default, the playbook runs on all managed nodes as it targets the `all` Ansible group. To run the installation playbook, issue the following command:

  `ansible-playbook lmt_install_or_upgrade_scanner.yml -i lmt_inventory.yml`

  Where `lmt_inventory.yml` is your inventory file. You can create the file by using the `lmt_inventory_template.yml` template.

   
  
- To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on the `unix1` host and the `windows` group of hosts, run the following command:

  `ansible-playbook lmt_install_or_upgrade_scanner.yml -i lmt_inventory.yml -l lmt_server,localhost,unix1,windows` 

After the scanner is installed, the playbook performs initial configuration of the scanner and by default schedules weekly software scans and daily generation of results packages.
For more information about this and other playbooks, see: [License Metric Tool playbooks](doc_playbooks_list.md).

#### **Manual installation**

Another possible approach to installing disconnected scanners is the manual installation. Such method might be preferable when allowing Ansible to escalate privileges to the root or administrator level is not possible due to security reasons.
When the Ansible inventory is configured without root or administrator rights, only the `lmt_collect_results.yml` playbook is supported.

For detailed instructions for manual installation, see the following topics in the License Metric Tool documentation.
1. [Downloading the disconnected scanner package](https://ibm.biz/LMT_download_disc_package).
2. [Installing the scanner and gathering the initial data](https://ibm.biz/LMT_install_disc_scanner).
3. [Running software scans and gathering scan results](https://ibm.biz/LMT_run_disc_sw_scan).
> **Tip**: Schedule regular software scans. In case of weekly software scans, schedule daily creation of scan results packages.

### 4. Schedule daily collection of scan results

1. For a non-root or non-Administrator user of Ansible and in case when the installation was performed manually (not by using the installation playbook), assign the user with correct privileges to the directory with disconnected scan results.
   - **\[UNIX/Linux\]** Ensure that the user has the **rwx** privileges in the home directory of the disconnected scanner and in the directory with scan results packages (the `lmt_scanner_output_path_unix` directory).
   
   - **\[Windows\]** Ensure that the following requirements are met.
     - The user has the `List folder contents`, `Read` and `Write` privileges in the directory with scan results packages (the `lmt_scanner_output_path_windows` directory). To verify that the user has the right privileges, select the user, open the `Advanced Security Settings` tab and click `View`. 
     - The directory with scan result packages (the `lmt_scanner_output_path_windows` directory) has inheritance enabled. To enable inheritance, open the `Advanced Security Settings` tab, and click `Enable inheritance`. The inheritance should apply to `This folder, subfolder and files`. 

2. To run the playbook that collects scan results daily, issue the following command:

   `ansible-playbook lmt_collect_results.yml -i lmt_inventory.yml`

    The playbook is configured to run on all managed nodes as it targets the `all` Ansible group. To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on the `unix1` host and the `windows` group of hosts, run the following command:

   `ansible-playbook lmt_collect_results.yml -i lmt_inventory.yml -l lmt_server,localhost,unix1,windows` 
   
   **Example** 
   
   To schedule the command to run every day at 2:30 AM by using crontab, open the crontab.
   
   `crontab -e` 
   
   Then, add the following line to crontab configuration.
   
   `30 2 * * * "ansible-playbook <LMT_upload_playbook_files_directory>/lmt_collect_results.yml -i <LMT_upload_playbook_files_directory>/lmt_inventory.yml"`
   
   where `<LMT_upload_playbook_files_directory>` is the directory where the playbook files are stored.


### 5. Maintain the environment 

After you successfully configure your environment, monitor it on an ongoing basis and fix potential configuration problems if necessary (for example, expired keys or passwords). 
- You can monitor the environment on the License Metric Tool dashboard. For more information, see: [Dashboard](https://ibm.biz/LMT_dashboard) in the License Metric Tool documentation. Pay special attention to the `Delayed Data Upload` status on the [Deployment Health](https://ibm.biz/LMT_deployment_health) widget and the Outdated Scan status on the [Capacity Scan Health](https://ibm.biz/LMT_capacity_scan_health) widget.
- In case of problems with the disconnected scanner, you can use the <code>lmt_collect_troubleshooting_data</code> playbook to collect 
data such as logs or raw scan data for troubleshooting purposes. 
- Keep your environment up to date and upgrade the scanner to the latest version by using the `lmt_install_or_upgrade_scanner` playbook after you upgrade the License Metric Tool server. 

For detailed list of all playbooks with description and information which parameters are relevant for them, see: [License Metric Tool playbooks](doc_playbooks_list.md).<br>
   
