# Setting up a new Ansible environment

If Ansible is not yet implemented in your organization, start by installing and configuring it. Then, clone or download this Github repository. Next, configure connections to the managed nodes and create an inventory of these nodes.


## Procedure
1. Install Ansible. For detailed instructions, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).

    >**Note:** The minimal supported version of Ansible is **2.10.2**. However, it is recommended to use the latest version of Ansible that is available.

2. Clone or download the Github repository.

3. Configure connections to your managed nodes.
   - **\[UNIX/Linux\]** For more information, see: [Connecting to remote nodes](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#connecting-to-remote-nodes) in the Ansible documentation.
   - **\[Windows\]** For more information, see: [Setting up a Windows Host](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information) and [Windows WinRM configuration](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html) in the Ansible documentation.


4. To create an inventory of your managed nodes, you can use the [lmt_inventory_template.yml](../lmt_inventory_template.yml) template.
   
   a. In the `lmt_server` parameter, define connection settings to the host where the License Metric Tool server is installed.
     - If you are running the playbook on the host where the License Metric Tool server is installed, use the default settings.
     - If you are running the playbook on a different host, define the host connection parameters and remove the `ansible_connection: local` line. 

       **Example**
       ```
       lmt_server:
         ansible_host: 192.168.0.11
         ansible_user: ansible
       ```
     
   b. Define all managed nodes from which you want to collect scan results and their connection settings.
   
      **Example**

      The following example shows how to define a managed node with a default user.
      ```
      server1:
        ansible_host: 192.168.0.1
      ```
   c. Review the default settings of License Metric Tool (parameters that start with `lmt_`) and customize them if needed. For the list of parameters, see: [Parameters used in playbooks](doc_lmt_parameters.md).
       
     **Example**

     In the following example, the `c:\my\lmt_scanner\output` directory is specified as a disconnected scanner output directory on all Windows computers and the `/my/disconnected/datasource` directory is specified as a disconnected data source directory configured on the License Metric Tool server. The remaining parameters that are not defined use the default values.
     ```
     vars:
       lmt_scanner_output_path_windows: 'c:\\my\\lmt_scanner\\output'
       lmt_server_datasource_path: '/my/disconnected/datasource'
     ```

   The following example shows an inventory for a playbook that runs on the same host as the License Metric Tool server. The inventory consists of 4 managed nodes: 2 Linux nodes, 1 Solaris node with a custom user and a custom path to scan results packages and 1 Windows node.
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
       lmt_scanner_output_path_windows: 'c:\\my\\lmt_scanner\\output'
       lmt_server_datasource_path: '/my/disconnected/datasource'
   ```
    For more information about inventories, see: [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) in the Ansible documentation.

<br>

## What to do next

Run the `lmt_install_or_upgrade_scanner` playbook to install the disconnected scanner and perform its initial configuration. For more information, see: [Install the disconnected scanner and schedule scans](doc_install_scanner.md).