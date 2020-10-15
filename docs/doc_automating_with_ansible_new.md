# Setting up a new Ansible environment

1. Install Ansible. For detailed instructions, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).

1. Clone or download the Github repository: [https://github.com/IBM/ansible-automation-for-lmt](https://github.com/IBM/ansible-automation-for-lmt). To clone the repository, run the following command:

   `git clone https://github.com/IBM/ansible-automation-for-lmt.git`


2. Configure connections to your managed nodes.
   - **\[UNIX/Linux\]** For more information, see: [Connecting to remote nodes](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#connecting-to-remote-nodes) in the Ansible documentation.
   - **\[Windows\]** For more information, see: [Setting up a Windows Host](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information) and [Windows WinRM configuration](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html) in the Ansible documentation.


3. To create an inventory of your managed nodes, edit the `lmt_disconnected_scans_inventory.yml` inventory file.
   
   - In the `lmt_server` parameter, define connection settings to the host where the License Metric Tool server is installed.
     - If you are running the playbook on the host where the License Metric Tool server is installed, use the default settings.
     - If you are running the playbook on a different host, define the host connection parameters and remove the `ansible_connection: local` line. 

       **Example**
       ```
       lmt_server:
         ansible_host: 192.168.0.11
         ansible_user: ansible
       ```
     
   - Define all managed hosts from which you want to collect scan results and their connection settings.
   
      **Example**

      The following example shows how to define a managed host with a default user:
      ```
      server1:
        ansible_host: 192.168.0.1
      ```
   - Review the default settings of License Metric Tool (parameters that start with `lmt_`) and customize them if needed. For the list of parameters, see: [License Metric Tool parameters](doc_lmt_parameters.md).
       
     **Example**

     In the following example, the `c:\my\lmt_scanner\output` directory is specified as a disconnected scanner output directory on all Windows computers and the `/my/disconnected/datasource` is specified as a disconnected data source directory configured on the License Metric Tool server. The remaining parameters use the default values.
     ```
     vars:
       lmt_local_file_storage_path: ''
       lmt_scanner_output_path_windows: 'c:\\my\\lmt_scanner\\output'
       lmt_scanner_output_path_unix: ''
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
       lmt_local_file_storage_path: ''
       lmt_scanner_output_path_windows: 'c:\\my\\lmt_scanner\\output'
       lmt_scanner_output_path_unix: ''
       lmt_server_datasource_path: '/my/disconnected/datasource'
   ```
    For more information about inventories, see: [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) in the Ansible documentation.
   
4. For a non-root or non-Administrator user of Ansible, assign the user with correct privileges to the directory with disconnected scan results.
   - **\[UNIX/Linux\]** Ensure that the user has the **rwx** privileges in the home directory of the disconnected scanner and **rw** privileges in the directory with scan results packages (the `lmt_scanner_output_path_unix` directory).
   
   - **\[Windows\]** Ensure that the following requirements are met.
     - The user has the `List folder contents`, `Read` and `Write` privileges in the directory with scan results packages (the `lmt_scanner_output_path_windows` directory). To verify that the user has the right privileges, select the user, open the `Advanced Security Settings` tab and click `View`. 
     - The directory with scan result packages (the `lmt_scanner_output_path_windows` directory) has inheritance enabled. To enable inheritance, open the `Advanced Security Settings` tab, and click `Enable inheritance`. The inheritance should apply to `This folder, subfolder and files`. 

5. To run the playbook every day, schedule the following command.

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml`

    The playbook is configured to run on all computers as it targets the `all` Ansible group. To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on unix1 host and windows group of hosts run the following command.

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml -l lmt_server,localhost,unix1,windows` 
   
   **Example** 
   
   To schedule the command to run every day at 2:30 AM by using crontab, open the crontab.
   
   `crontab -e` 
   
   Then, add the following line to crontab configuration.
   
   `30 2 * * * "ansible-playbook <LMT_upload_playbook_files_directory>/lmt_disconnected_scans_collector.yml -i <LMT_upload_playbook_files_directory>/lmt_disconnected_scans_inventory.yml"`
   
   where `<LMT_upload_playbook_files_directory>` is the directory where the playbook files are stored.
