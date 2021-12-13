# 4. Maintain the environment 

After you successfully configure your environment, monitor it on an ongoing basis and fix potential configuration problems if necessary (for example, if the keys or passwords expire). 

<br>

## Procedure
- You can monitor the environment on the License Metric Tool dashboard. For more information, see: [Dashboard](https://ibm.biz/LMT_dashboard) in the License Metric Tool documentation. Pay special attention to the `Delayed Data Upload` status on the [Deployment Health](https://ibm.biz/LMT_deployment_health) widget and the Outdated Scan status on the [Capacity Scan Health](https://ibm.biz/LMT_capacity_scan_health) widget.
- In case of problems with the disconnected scanner, you can use the [`lmt_collect_troubleshooting_data`](doc_playbooks_list.md#lmt_collect_troubleshooting_data) playbook to collect data such as logs or raw scan data for troubleshooting purposes. 
- Keep your environment up to date and upgrade the scanner to the latest version by using the [`lmt_install_or_upgrade_scanner`](doc_playbooks_list.md#lmt_install_or_upgrade_scanner) playbook after you upgrade the License Metric Tool server. 

For detailed list of all playbooks with description and information which parameters are relevant for them, see: [Playbooks](doc_playbooks_list.md).