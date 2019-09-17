# ansible-automation-for-lmt
Ansible Automation for IBM LMT

# Installation

Step by step ansible installation:    
1. Install ansible 2.8.2.  
    If you are running Red Hat Enterprise Linux (TM), CentOS, Fedora, Debian, or Ubuntu, we recommend using the OS package manager.

    For other installation options, we recommend installing via pip, which is the Python package manager.
    Read more about ansible requirements and installation [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#what-version-to-pick)

2. Ansible works with SSH, so it is required to set it up. Read more about it [here](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)

3. Install and schedule scanners on your hosts.  
   If you want use ansible to do it read more about it [here](#installer.yml) 




# Scans collecting
scans_collector.yml collects scans and uploads it to lmt server  

Setup:
1. Edit lmt_disconnected_scans_inventory.yml.

2. Check your connection to hosts with:
```
    ansible -m ping all -i lmt_disconnected_scans_inventory.yml
```

3. Run scans collector using
```
    ansible-playbook /path/to/playbook/lmt_disconnected_scans_collector.yml -i /path/to/inventory/lmt_disconnected_scans_inventory.yml
```
or schedule it by cron


## lmt_disconnected_scans_inventory.yml
It is required to prepare a correct inventory.  
Inventory must contains 2 gropus: 
+ endpoints
+ lmtserver - which contains variable lmt_datasource_path where you specify path to datasource folder specified in LMT Server     
[Working with inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)  
Example:
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
        #Custom name 
        endpoint1:
          ansible_host: <ip_address_of_endpoint1> 
          ansible_user: <username_for_endpoint1>
          #Absolute path to disconnected scanners output directory for specific endpoint
          scanner_output_path: /path/to/disconnected_scanners/outputs/
        endpoint2:
          ansible_host: <ip_address_of_endpoint2>
          ansible_user: <username_for_endpoint2>

      vars:
        #Absolute path to disconnected scanners output directory for all endpoints without specified path
        scanner_output_path: /path/to/disconnected_scanners/outputs/
    win_endpoints:
    #not supported yet
...
```
If you want to hash your passwords with ansible you should use ansible-vault. It is a tool designed to provide safe login with passwords or to use for sudo command.  
To create hashed file use:
```
ansible-vault create passwd.yml
```

After setting password you should edit passwd.yml with following syntax:
```
host1passwd: password1
host2passwd: password2
```
After saving file your data would be hashed.  
To edit it use:
```
ansible-vault edit passwd.yml
```

