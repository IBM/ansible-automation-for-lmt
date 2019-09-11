naging disconnected scans with Ansible


## Disconnected scans
Disconnected scans are an alternative way of discovering software and hardware inventory in your infrastructure. It is a BigFix-less solution which does not require the connection between the scanned computers and the BigFix server. The scripts designed for the diconnected scanner initiate software and capacity scans, and prepare scan results that you later on upload to License Metric Tool.


>**Note:** For more detailed description, see: [Discovering software and hardware with disconnected scanner on Windows and UNIX](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/c_disc_sys_main.html)




## Automating diconnected scans with Ansible


[**Ansible**](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. You can use it to automate the management of the disconnected scanners by performing the following configuration steps.
1. [Configure Ansible to manage the selected computers](###Configure-Ansible-to-manage-the-selected-computers).  
2. [Instrument the disconnected scanners to periodically scan the selected computers and generate output packages](###Instrument-disconnected-scanners-to-periodically-scan-the-selected-computers-and-generate-output-packages).
3. Define the disconnected data source.


### Configure Ansible to manage the selected computers 
To use Ansible for automation, you need a control node where you can run the Ansible playbook. The control node might be on the same machine as the LMT server. Control node comunicates with the managed notes (endpoints) and collects the disconnected scanner output.


**Requirements**
- Ansible can run on any host where Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) is installed. 
- Control node cannot run on Windows.
- For a full list of requirements, check [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements).


1. [Istall Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).
2. [Choose the most suitable connection method](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information).
3. Optional: If you want to create a dedicated user for Ansible to increase security, make sure that this user has the **rwx** privileges in the home directory and **rw** privileges in directory with the disconnected scanner package.
4. Clone the Github repository. (name)
5. Edit the `/etc/ansible/hosts`inventory, or create a new inventory.
The inventory needs to consist of two sections: endpoints and lmtserver. 
- Define managed nodes in the endpoints section. Provide the following information for each endpoint:
    -  `ansible_host`- an IP address of an endpoint
    - `ansible_user`- a username
    - `scanner_output_path`- define a path where the scan packages are stored on an endpoint
-   Define the LMT server in the lmtserver section. Provide the following information:
    - `ansible_host`- ?
    - `ansible_user`- ?
    - `lmt_datasource_path`- define a disconnected data source on the LMT server
    - `ansible_connection`- set the parameter to `local`if the control node is located on the same machine as the LMT server.
Example: file in repo.
6. Save the changes.
7. Run the scans_collector scripr from the repository with the following command:
8. 
- If you use the`/etc/ansible/hosts` inventory run: `ansible-playbook lmt_disconnected_scans_collector.yml`
- If you use an inventory file that you created run: `ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml/inventory_file_path`


### Instrument the disconnected scanners to periodically scan the selected computers and generate output packages




