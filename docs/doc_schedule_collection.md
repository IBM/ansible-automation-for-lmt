# 3. Schedule daily collection of scan results
After you install and configure the disconnetced scanner, schedule daily collection of scan results to ensure fresheness of data that is available in License Metric Tool.

<br>

## Procedure

1. For a non-root or non-Administrator user of Ansible, assign the user with correct privileges to the directory with disconnected scan results.
   - **\[UNIX/Linux\]** Ensure that the user has the **rwx** privileges in the home directory of the disconnected scanner and in the directory with scan results packages (the `lmt_scanner_output_path_unix` directory).
   
   - **\[Windows\]** Ensure that the following requirements are met.
     - The user has the `List folder contents`, `Read` and `Write` privileges in the directory with scan results packages (the `lmt_scanner_output_path_windows` directory). To verify that the user has the right privileges, select the user, open the `Advanced Security Settings` tab and click `View`. 
     - The directory with scan result packages (the `lmt_scanner_output_path_windows` directory) has inheritance enabled. To enable inheritance, open the `Advanced Security Settings` tab, and click `Enable inheritance`. The inheritance should apply to `This folder, subfolder and files`. 

2. To run the playbook that collects scan results daily, issue the following command:

   `ansible-playbook lmt_collect_results.yml -i lmt_inventory.yml`

    The playbook is configured to run on all managed nodes as it targets the `all` Ansible group. To run the playbook on specific hosts or groups only, use the `-l` switch in the command followed by `lmt_server,localhost` and the list of your hosts or host groups. For example, to target the playbook on the `unix1` host and the `windows` group of hosts, run the following command:

   `ansible-playbook lmt_collect_results.yml -i lmt_inventory.yml -l lmt_server,localhost,unix1,windows` 
   
   **Example** 
   
   To schedule the command to run every day at 2:30 AM by using crontab, open the crontab.
   
   `crontab -e` 
   
   Then, add the following line to crontab configuration.
   
   `30 2 * * * "ansible-playbook <LMT_upload_playbook_files_directory>/lmt_collect_results.yml -i <LMT_upload_playbook_files_directory>/lmt_inventory.yml"`
   
   where `<LMT_upload_playbook_files_directory>` is the directory where the playbook files are stored.

## What to do next
[Maintain the environment](doc_maintain_environment.md).