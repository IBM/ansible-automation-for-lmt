
# Managing disconnected scans with Ansible

## Disconnected scans

Disconnected scans are an alternative way of discovering software and hardware inventory in your infrastructure which does not require the connection between the scanned computers and the BigFix server. The scripts designed for the disconnected scanner initiate software and capacity scans, and prepare scan results that you later on upload to License Metric Tool.
You can use Ansible to automate the process of uploading the scan results from the endpoints to the LMT server.

>**Note:** For more detailed description, see: [Discovering software and hardware with disconnected scanner on Windows and UNIX](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/c_disc_sys_main.html).

## Automating collection of the disconnected scan results with Ansible 

[**Ansible**](https://docs.ansible.com/ansible/latest/index.html#about-ansible) is an open source automation tool that is used to automate applications and IT tasks. You can use it to automate the management of the disconnected scanners by performing the following configuration steps.


1. [Define the disconnected data source](#define-the-disconnected-data-source)

1. [Install and setup disconnected scans](#install-and-setup-disconnected-scans).

1. [Configure Ansible to manage the selected computers](#configure-ansible-to-manage-the-selected-computers).

### Define the disconnected data source

Perform the appropriate actions to [define the disconnected data source](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_sys_datasource.html).   

### Install and setup disconnected scans

Perform the appropriate actions to [download and configure the disconnected scanner](https://www.ibm.com/support/knowledgecenter/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/planinconf/t_disc_sys_downloading.html).

> **Tip**:Remember about scheduling regular software scans!

### Configure Ansible to manage the selected computers

To use Ansible for automation, you need a control node where you can run the Ansible playbook. The control node might be on the same machine as the LMT server. Control node communicates with the managed notes (endpoints) and collects the disconnected scanner output.

**Requirements**

- Ansible can run on any host where Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) is installed.

- Control node cannot run on Windows.

- For a full list of requirements, see: [Requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements).

**Procedure**

1. Clone or download the Github repository.

2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node).

3. Open and edit the `lmt_disconnected_scans_inventory.yml` file.
- Edit lmtserver host
    - If you are running Ansible playbook on the same host where LMT Server is installed, all you have to do is choose proper path for a disconnected datasource.
    - If you are running Ansible playbook on a different host where LMT Server is installed, you have to remove the line with `ansible_connection` parameter and define proper `ansible_user`(username) and `ansible_host`(Adders IP)

- Prepare unix_endpoints group 
    - Define endpoints from which you want to collect disconnected scanner result packages
    - You can define `scanner_output_path` in vars section for all endpoints, or for each endpoint separately.
    >**Note:** `scanner_output_path` is a path to directory with scan results not to a directory where disconnected scanner is installed. 

- Read more about inventories [here](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

Exemplary inventory for playbook running on the same host as LMT Server:
```
---
all: 
  hosts:
    lmtserver:
      ansible_host: localhost
      ansible_connection: local
      lmt_datasource_path: /opt/ibm/LMT/temp/
    children:
      unix_endpoints:
        hosts:
          endpoint1:
            ansible_host: 192.168.0.2
            ansible_user: ansible
          endpoint2:
            ansible_host: 192.168.0.4
            ansible_user: user1
  vars:
    scanner_output_path: /home/ansible/disconnected-scanner/output
...
```
>If you run the scripts on a different host than LMT Server, make appropriate changes in the lmtserver section, for example:

```
lmtserver:
  ansible_host: 192.168.0.11
  ansible_username: ansible
  lmt_datasource_path: /opt/ibm/LMT/temp/
```

4. [Choose the most suitable connection method](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#remote-connection-information).

5. Optional: If you want to create a dedicated user for Ansible to increase security, make sure that this user has the **rwx** privileges in the home directory and **rw** privileges in directory with the disconnected scanner package.

6. Add the following command to Crontab:

`ansible-playbook lmt_disconnected_scans_collector.yml -i lmt_disconnected_scans_inventory.yml`

___

## Using AWX or Tower to manage Disconnected Scanner

AWX is the open source, easy-to-use UI, dashboard and REST API for Ansible. 
Find out more about AWX [here](https://github.com/ansible/awx) 

Ansible Tower is a commercial version of AWX supported by Red Hat.
Find out more about Tower [here](https://www.ansible.com/products/tower)

1.  Install AWX or Tower.

2.  Create New Project with SCM TYPE as Git and provide URL to our repo.

3.  Create New Inventory. It needs the same structure as Inventory in repo: 
lmtserver, unix_endpoints group and endpoints inside this groups. Additionally, provide all required variables.

4.  Choose Credential type and define it.

5.  Create Job Template. Name it, provide inventory, credentials, project and then choose lmt_disconnected_scans_collector.yml as a playbook.

6. Schedule the job or lunch it manually.
