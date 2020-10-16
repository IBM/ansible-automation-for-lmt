# License Metric Tool parameters

Parameter | Default value | Description 
--------- | ------------- | -----------
|`lmt_local_file_storage_path` | `./lmt_file_storage` <p>It is a subdirectory of the directory where the `lmt_disconnected_scans_collector.yml` playbook is located.</p> | Path on a control node where the License Metric Tool files are stored. It will contain packages with disconnected scan results fetched from all computers. Do not place the directory in a temporary location (e.g. `/tmp`) as this is expected to be a persistent storage of License Metric Tool files. |
| `lmt_scanner_output_path_windows` | \<ProgramFiles>\IBM\LMTScanner\output | Path to a directory where packages with scan results are generated on Windows. |
| `lmt_scanner_output_path_unix`| /var/opt/ibm/lmt_scanner/output | Path to a directory where packages with scan results are generated on UNIX/Linux. |
| `lmt_server_datasource_path` | /opt/ibm/LMT/temp | Path to a disconnected data source directory on the License Metric Tool server. |
