# 1. Configure Ansible to manage the selected computers

To use Ansible for automation, you need a control node where you can run Ansible playbooks. The control node communicates with managed nodes (computers scanned by the disconnected scanner) and collects scan results. The control node can be on the same computer as the License Metric Tool server or on a different one. 

<br>

## Requirements


<details>
<summary>Supported version of Ansible</summary>

The minimal supported version of Ansible is 2.10.2. However, it is recommended to use the latest version of Ansible that is available.

</details>

<details>
<summary>Control node requirements</summary>

  - Python 2.7, or Python 3.5 or a higher 3.x version. 
  - [`pywinrm`](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#what-is-winrm) package installed to communicate with Windows servers over WinRM.
  - For a full list of control node requirements and the most up-to-date information, see: [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements) in the Ansible documentation.
	>**Note:** Control node is not officially supported on Windows.
</details>

<details>
<summary>Managed node requirements</summary>

  **\[UNIX/Linux\]** 
  - A way to communicate which usually is SSH. By default, this uses SFTP. If that is not available, you can switch to SCP in the `ansible.cfg`. 
  - Python 2.6 or a higher 2.x version, or Python 3.5 or a higher 3.x version.
  - The `lmt_install_or_upgrade_scanner` and `lmt_collect_troubleshooting_data` playbooks require the `gzip` and `tar` commands to be present in the <code>$PATH</code> of the configured Ansible user.
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.

    >**Note:** If you have SELinux enabled on the License Metric Tool server (the `lmt_server` host) and the server is on a managed node, install `libselinux-python` to be able to copy the disconnected scan results. For more information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) in the Ansible documentation.
    
  **\[Windows\]** 
  - Ansible can generally manage Windows versions that are under current and extended support from Microsoft. Ansible can manage desktop operating systems including Windows 7, 8.1, and 10, and server operating systems including Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016, and 2019.
  - PowerShell 3.0 or higher and at least .NET 4.0 must be installed on the host.
  - A WinRM listener must be created and activated.
  - The user must be a member of the local Administrators group or must be explicitly granted access.
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements on Windows](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#windows-setup) in the Ansible documentation.
</details>

<details>
<summary>License Metric Tool requirements for managed nodes</summary>

  - For the list of disconnected scanner requirements, see: [IBM License Metric Tool - Supported Operating Systems](https://ibm.biz/LMT_supported_OS). On the page that opens, click Disconnected scanner.
</details>

<br>

## Procedure

- If Ansible is not yet implemented in your organization, see: [Setting up a new Ansible environment](doc_automating_with_ansible_new.md).
- If you already have Ansible implemented, see: [Setting up an existing Ansible environment](doc_automating_with_ansible_existing.md).
