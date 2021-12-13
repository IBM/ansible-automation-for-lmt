# 2. Install the disconnected scanner and schedule scans

To install the disconnected scanner on your managed nodes, run the `lmt_install_or_upgrade_scanner` playbook. The playbook also performs initial configuration of the scanner. By default, it schedules weekly software scans and daily generation of packages with scan results.

<br>

## Requirements

- Before you run this playbook, review the default configuration parameters and adjust them as needed. For more information, see: [Parameters used in playbooks](doc_lmt_parameters.md). 
- This playbook requires full root or administrator rights. Ensure that you configure valid privilege escalation in the Ansible inventory or use root or administrator accounts directly.

<br>

## Procedure
- By default, the playbook runs on all managed nodes as it targets the `all` Ansible group. To run the installation playbook, issue the following command:

  `ansible-playbook lmt_install_or_upgrade_scanner.yml -i lmt_inventory.yml`

  Where `lmt_inventory.yml` is your inventory file. You can create the file by using the [lmt_inventory_template.yml](../lmt_inventory_template.yml) template.

   
  
- To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on the `unix1` host and the `windows` group of hosts, run the following command:

  `ansible-playbook lmt_install_or_upgrade_scanner.yml -i lmt_inventory.yml -l lmt_server,localhost,unix1,windows` 

<br>

 ## What to do next 
 [Schedule daily collection of scan results](doc_schedule_collection.md). 