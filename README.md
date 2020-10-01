
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

- Tested on Ansible version 2.8.

- Control node requirements
  - Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) installed. This includes Red Hat, Debian, CentOS, macOS, any of the BSDs, and so on.
  
  - For a full list of control nodes requirements, see: [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements).
  
  >**Note:** Control node cannot run on Windows.

- \[**Unix**\] Managed node requirements

  - On the managed nodes, you need a way to communicate, which is normally SSH. By default this uses SFTP. If that’s not available, you can switch to SCP in ansible.cfg. You also need Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later).

  - For a full list of managed nodes requirements, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements).

  >**Note:** If you have SELinux enabled on LMT Server and it's not a control node, but a managed node, you will have to install libselinux-python on it to be able to copy the disconnected scanner outputs, see: [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements) for details.
    
- \[**Windows**\] Managed node requirements
    - Ansible can generally manage Windows versions under current and extended support from Microsoft. Ansible can manage desktop OSs including Windows 7, 8.1, and 10, and server OSs including Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016, and 2019.
    - Ansible requires PowerShell 3.0 or newer and at least .NET 4.0 to be installed on the Windows host.
    - A WinRM listener should be created and activated.

    - For a full list of managed nodes requirements, see: [Managed node requirements on Windows](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#windows-setup).


**Procedure for a new environment setup for Ansible**

1. Clone or download the Github repository.

2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).

3. Open and edit the `lmt_disconnected_scans_inventory_new_ansible_setup.yml` file.
   - Edit lmt_server host
     - If you are running Ansible playbook on the same host where LMT Server is installed, all you have to do is choose proper path for a disconnected datasource.
     - If you are running Ansible playbook on a different host where LMT Server is installed, you have to remove the line with `ansible_connection` parameter and define proper `ansible_user`(username) and `ansible_host`(ip address)

     >If you run the scripts on a different host than LMT Server, make appropriate changes in the lmt_server section, for example:

     ```
     lmt_server:
       ansible_host: 192.168.0.11
       ansible_username: ansible
       lmt_datasource_path: /opt/ibm/LMT/temp/
     ```

   - Prepare lmt_scanners_unix and lmt_scanners_windows groups
     - Define endpoints, which you want to collect disconnected scanner result packages from.
     - You can define `scanner_output_path` in vars section for all Unix/Windows endpoints, or for each endpoint separately.
     >**Note:** `scanner_output_path` is a path to a directory with scan results not to a directory where the disconnected scanner is installed. 

   - Read more about [inventories](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

   Exemplary inventory for a playbook running on the same host as LMT Server:
   ```
   ---
   all: 
     hosts:
       lmt_server:
         ansible_host: localhost
         ansible_connection: local
         lmt_datasource_path: /opt/ibm/LMT/temp/
       children:
         lmt_scanners_unix:
           hosts:
             unix_endpoint1:
               ansible_host: 192.168.0.1
               ansible_user: ansible
             unix_endpoint2:
               ansible_host: 192.168.0.2
               ansible_user: user1
           vars:
             scanner_output_path: /home/ansible/disconnected-scanner/output
          
         lmt_scanners_windows:
           hosts:
             windows_endpoint1:
               ansible_host: 192.168.0.3
               ansible_user: ansible
               ansible_password: secret_pass1
          
           vars:
             scanner_output_path: c:\path\to\disconnected_scanners\outputs
        
             ansible_port: 5986
             ansible_connection: winrm
             ansible_winrm_transport: ntlm

   ```

4. \[**Unix**\] [Choose the most suitable connection method](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information).

5. \[**Unix**\] Optional: If you want to create a dedicated user for Ansible to increase security, make sure that this user has the **rwx** privileges in the home directory and **rw** privileges in a directory with the disconnected scanner package.

6. To run the playbook, add the following command to Crontab on a control node:

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory_new_ansible_setup.yml`

**Procedure for an existing Ansible environment**

1. Clone or download the Github repository.

2. Open and edit the `lmt_disconnected_scans_inventory_existing_ansible_setup.yml` file.
   - Edit lmt_server host
     - If you are running Ansible playbook on the same host where LMT Server is installed, all you have to do is choose proper path for a disconnected datasource.
     - If you are running Ansible playbook on a different host where LMT Server is installed, you have to remove the line with `ansible_connection` parameter and define proper `ansible_user`(username) and `ansible_host`(ip address)

     >If you run the scripts on a different host than LMT Server, make appropriate changes in the lmt_server section, for example:

     ```
     lmt_server:
       ansible_host: 192.168.0.11
       ansible_username: ansible
       lmt_datasource_path: /opt/ibm/LMT/temp/
     ```

   - Prepare lmt_scanners_unix and lmt_scanners_windows groups
     - Specify your existing groups (under 'children') and/or existing endpoints (under 'hosts') from which you want to collect disconnected scanner result packages.
     - You can define `scanner_output_path` in vars section for all Unix/Windows endpoints, or for each endpoint separately.
     >**Note:** `scanner_output_path` is a path to a directory with scan results not to a directory where the disconnected scanner is installed. 

   Exemplary inventory for a playbook running on the same host as LMT Server:
   ```
   ---
   all: 
     hosts:
       lmt_server:
         ansible_host: localhost
         ansible_connection: local
         #Path to disconnected datasource in LMT Server
         lmt_datasource_path: /opt/ibm/LMT/temp/

     children:
       lmt_scanners_unix:
         hosts:
           your_exisiting_unix_endpoint1:
           your_exisiting_unix_endpoint2:

         children:
           your_exisiting_unix_group1:

         vars:
           #Absolute path to disconnected scanners output directory for all Unix endpoints
           scanner_output_path: /path/to/disconnected_scanners/outputs
        
       lmt_scanners_windows:
         hosts:
           your_exisiting_windows_endpoint1:

         children:
           your_exisiting_windows_group1:
  
         vars:
           #Absolute path to disconnected scanners output directory for all Windows endpoints
           scanner_output_path: c:\path\to\disconnected_scanners\outputs

     ```

3. Prepare your existing inventory file (or choose a different method of providing your inventory), where your existing groups and/or endpoints you added to lmt_scanners_unix and/or lmt_scanners_windows groups are defined.

4. To run the playbook, add the following command to Crontab on a control node:

   `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory_existing_ansible_setup.yml -i <your_existing_inventory_file_name>`
___

## Using AWX or Tower to manage Disconnected Scanner

AWX is the open source, easy-to-use UI, dashboard and REST API for Ansible. 
Find out more about AWX [here](https://github.com/ansible/awx).

Ansible Tower is a commercial version of AWX supported by Red Hat.
Find out more about Tower [here](https://www.ansible.com/products/tower).

1.  Install AWX or Tower.

2.  Create New Project with SCM TYPE as Git and provide URL to our repo.

3.  Create New Inventory. It needs the same structure as Inventory in the repo: 
lmt_server, lmt_scanners_unix and/or lmt_scanners_windows groups and your endpoints and/or groups inside these groups. Additionally, provide all required variables.

4.  Choose Credential type and define it.

5.  Create Job Template. Name it, provide inventory, credentials, project and then choose lmt_disconnected_scans_collector.yml as a playbook.

6. Schedule the job or lunch it manually.
