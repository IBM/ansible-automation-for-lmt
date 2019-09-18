# Ansible Automation for IBM LMT

### Requirements
 - This script needs to run on the same machine where LMT Server >= 9.2.17 is installed.
 - We need to provide user with sudo access on endpoint
 - You can use Ansible Vault to store sudo passwords
 - In the simplest scenario when we have one endpoint or sudo pass are the same on every machine we can use --ask-become flag in command and provide sudo pass manually

### LMT Disconnected scanner installation with Ansible - step by step:


1. Install Ansible
1. Prepare ssh connection and credentials to your endpoint
1. Prepare your inventory - provide endpoint address and username, you can add additional endpoints, provide required variables.
    Variables:
    - disconnected_scanner_installation_path - installation path fo disconnected scanner
    - lmt_server_installation_path - path to LMT Server directory 
    - SW_SCAN_SCHEDULE_ENABLED - disconnected scanner option - TRUE/FALSE
    - SW_SCAN_FREQUENCY - disconnected scanner option - WEEKLY/DAILY
    - PACKAGE_OUTPUT_DIR - disconnected scanner option - Possible relative and absolute path, relative from Disconnected Scanner installation path
    - disconnected_scanner_for_linux solaris etc... - you can provide path to disconnected scanner on your own
1. Run playbook using

`ansible-playbook lmt_disconnected_scanner_installer.yml -i inventory --ask-become`