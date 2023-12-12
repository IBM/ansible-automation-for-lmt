# Managing License Metric Tool disconnected scans with Ansible

Disconnected scans allow for discovering software and hardware inventory by running scripts that are provided in the disconnected scanner package. The scripts initiate software and capacity scans, and create a package with scan results that you later need to upload to the License Metric Tool server. You can automate management of disconnected scans by using [Ansible](https://docs.ansible.com/ansible/latest/index.html#about-ansible).

The current version of License Metric Tool playbooks is **9.2.34**


## Prerequisites

- **Supported version of Ansible**

    Ensure that you have Ansible in version **2.10.2** or higher. If you have an earlier version of Ansible, upgrade your environment, preferably to the latest available version. Otherwise, License Metric Tool playbooks will not work.

- **Supported version of the disconnected scanner**

    The minimal supported version of License Metric Tool disconnected scanner to use with Ansible automation is **9.2.22**.

- **Supported operating systems**

    Playbooks that are delivered with License Metric Tool are supported on AIX, Linux, Solaris and Windows. They are not supported on IBM i. For information about exact versions, see the [list of operating systems that are supported by the disconnected scanner](https://www.ibm.com/support/pages/node/561443). 


<br>

## Overview

After you install License Metric Tool, set up Ansible to manage disconnected scans by using playbooks. If Ansible is not yet implemented in your organization, refer to Ansible documentation for information how to install and configure it. If you already have Ansible implemented, ensure that it meets all requirements of License Metric Tool and configure it to manage the selected computers. 

After you configure Ansible, run the `lmt_install_or_upgrade_scanner` playbook to install the disconnected scanner on the managed nodes. The playbook also performs initial configuration of the scanner. By default, it schedules weekly software scans and daily generation of packages with scan results. Next, run the `lmt_collect_results` playbook to schedule daily collection of scan results from the managed nodes. 

<br>

## Table of contents


- [Managing disconnected scans with Ansible](docs/doc_automating_with_ansible.md)
    1. [Configure Ansible to manage the selected computers](docs/doc_configure_ansible.md)
    2. [Install the disconnected scanner and schedule scans](docs/doc_install_scanner.md)
    3. [Schedule daily collection of scan results](docs/doc_schedule_collection.md)
    4. [Maintain the environment](docs/doc_maintain_environment.md)

- [Managing disconnected scans with AWX or Tower](docs/doc_automating_with_awx_tower.md)
- [License Metric Tool playbooks](docs/doc_playbooks_list.md)
    - [Parameters used in playbooks](docs/doc_lmt_parameters.md)
    - [Updating playbooks](docs/doc_updating_lmt_playbooks.md)
- [Release notes](docs/release_notes.md)    

<br>

## Other resources
- [Installing License Metric Tool with Ansible](https://www.ibm.com/docs/en/license-metric-tool?topic=installing-disconnected-scanners-ansible-lite) 
