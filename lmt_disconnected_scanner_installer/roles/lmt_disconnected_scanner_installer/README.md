Role Name
=========

Ansible automation for LMT Disconnected Scanners installation

Requirements
------------

This role require to be lunch on the same machine where LMT Server is installed.

Role Variables
--------------
Path to place where disconnected scanner should be installed 
disconnected_scanner_installation_path=/opt/ibm/Disconnected

Path to place where LMT Server is installed on local machine
lmt_server_installation_path=/opt/ibm/LMT/

Disconnected Scanner settings
SW_SCAN_SCHEDULE_ENABLED=TRUE
SW_SCAN_FREQUENCY=WEEKLY
PACKAGE_OUTPUT_DIR=./output

Exact path to disconnected scanner install package for every system. You can find them automatically using scanner_search.yml file in role.

disconnected_scanner_for_linux=/opt/ibm/LMT/disconnected-scanners/LMT-DisconnectedScanner-linux-9.2.16.0-20190805-1420.tar.gz
disconnected_scanner_for_solaris=/opt/ibm/LMT/disconnected-scanners/LMT-DisconnectedScanner-solaris-9.2.16.0-20190805-1420.tar.gz
disconnected_scanner_for_aix=/opt/ibm/LMT/disconnected-scanners/LMT-DisconnectedScanner-aix-9.2.16.0-20190805-1420.tar.gz
disconnected_scanner_for_hp_ux=/opt/ibm/LMT/disconnected-scanners/LMT-DisconnectedScanner-hp-ux-9.2.16.0-20190805-1420.tar.gz

