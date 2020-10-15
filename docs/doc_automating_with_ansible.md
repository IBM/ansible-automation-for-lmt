# Automating collection of disconnected scan results with Ansible 
[Ansible](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. To automate the management of the disconnected scanners with Ansible, perform the following steps.


### 1. Define a disconnected data source

For detailed instructions, see: [Adding a data source](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_datasource.html) in the License Metric Tool documentation.   

### 2. Install the disconnected scanner and schedule scans

For detailed instructions, see the following topics in the License Metric Tool documentation.
1. [Downloading the disconnected scanner package](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_downloading.html).
2. [Installing the scanner and gathering the initial data](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_setup_all.html).
3. [Running software scans and gathering scan results](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_scans_software.html).
> **Tip**: Remember to schedule regular software scans.

### 3. Configure Ansible to manage the selected computers

To use Ansible for automation, you need a control node where you can run the Ansible playbook. The control node communicates with the managed nodes (computers) and collects results of the disconnected scans. The control node can be on the same computer as the License Metric Tool server or on a different one. 

**Requirements**

<details>
<summary>Ansible version</summary>

The solution is tested on Ansible 2.10.2.

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
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.

    >**Note:** If you have SELinux enabled on the License Metric Tool server (the `lmt_server` host) and the server is on a managed node, install `libselinux-python` to be able to copy the disconnected scan results. For more information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.
    
  **\[Windows\]** 
  - Ansible can generally manage Windows versions that are under current and extended support from Microsoft. Ansible can manage desktop OSes including Windows 7, 8.1, and 10, and server OSes including Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016, and 2019.
  - PowerShell 3.0 or higher and at least .NET 4.0 must be installed on the Windows host.
  - A WinRM listener must be created and activated.
  - The user must be a member of the local Administrators group or must be explicitly granted access.
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements on Windows](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#windows-setup) in the Ansible documentation.
</details>

<details>
<summary>License Metric Tool requirements for managed nodes</summary>

  - For the list of disconnected scanner requirements, see: [Supported operating systems for ILMT 9.2.21 disconnected scanners](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/osForProduct?deliverableId=5CBD9B00A02711EA88F5BE8EBF8F323B&osPlatforms=AIX|HP|IBM%20i|Linux|Solaris|Windows&duComponentIds=A002).
</details>

**Procedure**

- If you are setting up a new Ansible environment, see: [Setting up a new Ansible environment](doc_automating_with_ansible_new.md).
- If you already have Ansible set up, see: [Setting up an existing Ansible environment](doc_automating_with_ansible_existing.md).
