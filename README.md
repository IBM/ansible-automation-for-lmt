
# Managing disconnected scans with Ansible

## Disconnected scans

Disconnected scans allow for discovering software and hardware inventory on computers that do not have connection to the BigFix server. Scripts that are provided in the disconnected scanner package initiate software and capacity scans, and create a package with scan results that you later upload to License Metric Tool.
You can use Ansible to automate the process of uploading the scan results from the endpoints to the LMT server.

>**Note:** For more detailed description, see: [Disconnected scan configuration](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/c_disc_main.html).

## Automating collection of the disconnected scan results with Ansible 

[**Ansible**](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. You can use it to automate the management of the disconnected scanners by performing the following configuration steps.


1. [Define the disconnected data source](#define-the-disconnected-data-source)

1. [Install and setup disconnected scans](#install-and-setup-disconnected-scans).

1. [Configure Ansible to manage the selected computers](#configure-ansible-to-manage-the-selected-computers).

### Define the disconnected data source

Perform the appropriate actions to [define the disconnected data source](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_datasource.html).   

### Install and setup disconnected scans

Perform the appropriate actions to [download and configure the disconnected scanner](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_downloading.html).

> **Tip**: Remember about scheduling regular software scans!

### Configure Ansible to manage the selected computers

To use Ansible for automation, you need a control node where you can run the Ansible playbook. The control node might be on the same machine as the LMT server. Control node communicates with the managed nodes (endpoints) and collects the disconnected scanner output.

**Requirements**

<details>
<summary>Tested on Ansible version 2.8.</summary>
Tested on Ansible version 2.8.
</details>

<details>
<summary>Control node requirements</summary>

  - Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) installed. This includes Red Hat, Debian, CentOS, macOS, any of the BSDs, and so on.
  
  - [`pywinrm`](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#what-is-winrm) package installed to communicate with Windows servers over WinRM
  
  - For a full list of control nodes requirements and the most up-to-date information, see: [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements).
  
  >**Note:** Control node is not officially supported on Windows.

</details>

<details>
<summary>Managed node requirements</summary>

  **\[Unix/Linux\]** 
  - a way to communicate, which is normally SSH. By default this uses SFTP. If that’s not available, you can switch to SCP in ansible.cfg. 
  - Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later).
  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements).

  >**Note:** If you have SELinux enabled on LMT Server (`lmt_server` host) and it's not running on a control node, but a managed node, you need `libselinux-python` installed to be able to copy the disconnected scanner outputs, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) for details.
    
  **\[Windows\]** 
  - Ansible can generally manage Windows versions under current and extended support from Microsoft. Ansible can manage desktop OSs including Windows 7, 8.1, and 10, and server OSs including Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016, and 2019.
  - Ansible requires PowerShell 3.0 or newer and at least .NET 4.0 to be installed on the Windows host.
  - A WinRM listener should be created and activated.
  - The user is a member of the local Administrators group or has been explicitly granted access.

  - For a full list of managed nodes requirements and the most up-to-date information, see: [Managed node requirements on Windows](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#windows-setup).

</details>

<details>
<summary>LMT requirements for managed nodes</summary>

  - ILMT disconnected scanner requirements must be met, see [Supported operating systems for ILMT 9.2.21 disconnected scanners](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/osForProduct?deliverableId=5CBD9B00A02711EA88F5BE8EBF8F323B&osPlatforms=AIX|HP|IBM%20i|Linux|Solaris|Windows&duComponentIds=A002)

</details>
  
**Procedure for a new environment setup of Ansible**

1. Clone or download the Github repository.

1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).

1. Create your inventory.

   - Define `lmt_server` host connection settings, where LMT server is installed.
     - If you are running the playbook on the same host where LMT Server is located, use the default settings.
     - If you are running the playbook on a different host than LMT Server, define `ansible_host`(hostname or ip address) and `ansible_user`(username) if it's different than the local user.

     Example of LMT Server located on a different host than the control node (192.168.0.11 in this example):
     ```
     lmt_server:
       ansible_host: 192.168.0.11
       ansible_user: ansible
     ```
     
   - Define all managed hosts from where you want to collect disconnected scanner scan results and their connection settings.
   
     Example of a machine with a default user:
     ```
     server1:
       ansible_host: 192.168.0.1
     ```
   - Review the default LMT settings (prefixed with `lmt_`) and if needed customize them to fit your environment. The settings, which are in `lmt_disconnected_scans_inventory.yml` inventory file, are the following:
     - `lmt_file_storage_path` (default ./lmt_file_storage, which is created as a subdirectory of a directory where the `lmt_disconnected_scans_collector.yml` playbook is located) - a path on a control node where LMT files are stored. It will contain disconnected scanner result packages fetched from all endpoints.
     - `lmt_scanner_output_path_windows` (default \<ProgramFiles\>\IBM\LMTScanner\output) - a disconnected scanner output path where scan result packages are generated on Windows
     - `lmt_scanner_output_path_unix` (default /var/opt/ibm/lmt_scanner/output) - a disconnected scanner output path where scan result packages are generated on Unix/Linux
     - `lmt_datasource_path` (default /opt/ibm/LMT/temp) - a path to a disconnected data source directory on LMT Server
       
     Example:
     ```
       vars:
         lmt_file_storage_path: /tmp/lmt_file_storage
         lmt_scanner_output_path_windows: C:\ilmt_disconnected\output
         lmt_scanner_output_path_unix: /ilmt_disconnected/output
         lmt_datasource_path: /opt/ibm/LMT/disconnected_data_source1
     ```

   - Read more about [inventories](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

   Exemplary inventory for a playbook running on the same host as LMT Server with 4 managed nodes defined (2 Linux, 1 Solaris with a custom user and a custom path to scan result packages and 1 Windows machines):
   ```
   ---
   all:
     hosts:
       lmt_server:
         ansible_host: localhost
         ansible_connection: local
      linux1:
        ansible_host: 192.168.0.1
      linux2:
        ansible_host: 192.168.0.2
      solaris1:
        ansible_host: 192.168.0.3
        ansible_user: different_user
        lmt_scanner_output_path_unix: /ilmt_disconnected/custom/path/output
      windows1:
        ansible_host: 192.168.0.4
        ansible_user: windows1_user
        ansible_password: windows1_password
        ansible_port: 5986
        ansible_connection: winrm
        ansible_winrm_transport: ntlm
        
        # not recommended, ignores certificate validation for self-signed certificates (test environments only)
        ansible_winrm_server_cert_validation: ignore

        # It an example for a test purposes only - Windows endpoints can be configured in a more secure manner, refer to the existing documentation for details:
        #https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#winrm-setup
        #https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html
     
     vars:
       lmt_file_storage_path: '/tmp/lmt_scan_file_storage'
       lmt_scanner_output_path_windows: 'C:\ilmt_disconnected\output'
       lmt_scanner_output_path_unix: '/ilmt_disconnected/output'
       lmt_datasource_path: '/opt/ibm/LMT/disconnected_data_source1'

       #
       # LMT default internal settings - DO NOT MODIFY
       # 
       
       # section removed for clarity - DO NOT REMOVE IT from the inventory file
   ```

1. Configure connections to managed nodes.
   - **\[Unix/Linux\]** [Connecting to remote nodes](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#connecting-to-remote-nodes)
   - **\[Windows\]** 
     - [Setting up a Windows Host](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information)
     - [Windows WinRM configuration](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html)

1. **\[Unix/Linux\]** Optional: If you want to create a dedicated user for Ansible to increase security, make sure that this user has the **rwx** privileges in the home directory and **rw** privileges in a directory with the disconnected scanner scan result packages (`lmt_scanner_output_path_unix` directory).

1. Schedule the following command to run the playbook everyday.

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml`
   
   E.g. to schedule the command to run everyday at 2:30 am in crontab, run 
   
   `crontab -e` 
   
   and edit the crontab configuration by adding the line:
   
   `30 2 * * * "ansible-playbook <LMT_upload_playbook_files_directory>/lmt_disconnected_scans_collector.yml -i <LMT_upload_playbook_files_directory>/lmt_disconnected_scans_inventory.yml"`
   
   where `<LMT_upload_playbook_files_directory>` is the directory where the playbook files are stored.

**Procedure for an existing Ansible environment**

1. Clone or download the Github repository.

1. Review the default LMT settings (prefixed with `lmt_`) and if needed customize them to fit your environment.

1. Schedule to run `lmt_disconnected_scans_collector.yml` playbook everyday.

___

## Using AWX or Tower to manage Disconnected Scanner

AWX is the open source, easy-to-use UI, dashboard and REST API for Ansible. 
Find out more about AWX [here](https://github.com/ansible/awx).

Ansible Tower is a commercial version of AWX supported by Red Hat.
Find out more about Tower [here](https://www.ansible.com/products/tower).

1.  Install AWX or Tower.

2.  Create New Project with SCM TYPE as Git and provide URL to our repo.

3.  Create New Inventory. It needs the same structure as the inventory in the repo with `lmt_server` host and the LMT settings defined (prefixed with `lmt_`).

4.  Choose Credential type and define it.

5.  Create Job Template. Name it, provide inventory, credentials, project and then choose lmt_disconnected_scans_collector.yml as the playbook.

6. Schedule the job to run everyday.
