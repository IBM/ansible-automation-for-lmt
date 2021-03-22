# License Metric Tool playbooks

License Metric Tool playbooks are supported on operating systems that are supported by the disconnected scanner except for **IBM i**. For a list of supported operating systems, see: [IBM License Metric Tool 9.2 - Supported Operating Systems](https://ibm.biz/LMT_supported_OS).

<table>

  <tr>
    <th rowspan="2">Name</th>
    <th>Required access</th>
    <th>Minimal set of targeted nodes</th>
  </tr>
  <tr>    
    <th colspan="4">Description</th>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_collect_results</code> 
    </td>
    <td>
        Read, execute and write access to the scanner output directories 
    </td>
    <td>
        <code>localhost</code><br/>
        <code>lmt_server</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        This playbook collects packages with disconnected 
        scan results from the following scanner output directories:<br/>
        <ul>
            <li><b>[UNIX/Linux]</b> <code>{{lmt_scanner_output_path_unix}}</code></li>
            <li><b>[Windows]</b> <code>{{lmt_scanner_output_path_windows}}</code></li>
        </ul>
        and stores them in the <code>{{lmt_local_file_storage_path}}/scan_result_packages</code> temporary folder on the control node. Then, it uploads the packages to the <code>{{lmt_server_datasource_path}}</code> directory on the <code>lmt_server</code> node.<br/>
        Finally, it removes collected scan results from the scanner output directories and from the temporary folder on the control node.       
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_install_or_upgrade_scanner</code> 
    </td>
    <td>
        Root / Local Administrator rights
    </td>
    <td>
        <code>localhost</code><br/>
        <code>lmt_server</code>
    </td>

  </tr>
  <tr>    
    <td colspan="4">
        This playbook installs or upgrades the disconnected scanner to the same version as the version of your License Metric Tool server.<br>
        <b>NOTE:</b> Before you run this playbook, review the default values of the <a href="doc_lmt_parameters.md">License Metric Tool parameters</a> and adjust them if needed.<br/>
        <p>
        <br>
        The following tasks are performed within this playbook:<br/>
        <br/>
        1. The latest version of the disconnected scanner packages is downloaded from the <code>{{lmt_server_path}}/disconnected_scanners</code> directory on the <code>lmt_server</code> node and is stored in the <code>{{lmt_local_file_storage_path}}/scanner_installers</code> folder on the control node.
        <br/><br/>
        2. The playbook checks whether the scanner is already installed or whether its version is older than the latest version that is available. Then, it either continues the installation or upgrade, or exits.
        <br/><br/>
        3. In case of the upgrade, the currently installed scanner is uninstalled. Its configuration from the <code>isotag_config.xml</code> and <code>sw_config.xml</code> scanner settings files,
        and the <code>endpoint_id.txt</code> file are preserved in the <code>config_backup</code> subdirectory of the disconnected scanner directory.
        <br/><br/>
        4. It is verified whether a sufficient amount of free disc space is available on the partition on which the scanner is going to be installed. If not, the installation is stopped. 
        The following amount of free disc space is required:</br>
        <ul>
            <li><b>[Linux]</b> 60 MB</li>
            <li><b>[Windows]</b> 80 MB</li>
            <li><b>[AIX]</b> 100 MB</li>
            <li><b>[HP-UX]</b> 100 MB</li>
            <li><b>[Solaris]</b> 100 MB</li>
        </ul>
        5. The disconnected scanner package that is suitable for the operating system of the managed node is uploaded to the following directory:<br/>
        <ul>
            <li><b>[UNIX/Linux]</b> <code>{{lmt_scanner_path_unix}}</code></li>
            <li><b>[Windows]</b> <code>{{lmt_scanner_path_windows}}</code></li>
        </ul>
        6. The scanner package is uncompressed by using the following method:<br/>
        <ul>
            <li><b>[UNIX/Linux]</b> <code style="color:red">gzip</code> and <code style="color:red">tar</code> commands that need to be present in the <code>$PATH</code> of the configured Ansible user.</li>
            <li><b>[Windows]</b> <code>win_unzip</code> Ansible module.</li>
        </ul>
        7. In case of the upgrade, the scanner configuration and endpoint ID are restored from backup.
        <br/><br/>
        8.  The <code>setup_config.ini</code> configuration file is configured according to the defined parameters and the following
        installation script is started:<br/>
        <ul>
            <li><b>[UNIX/Linux]</b> <code>{{lmt_scanner_path_unix}}/setup.sh</code></li>
            <li><b>[Windows]</b> <code>{{lmt_scanner_path_windows}}\setup.bat</code></li>
        </ul>
        9. It is verified that the installation completed successfully and all temporary or backup files are removed.
        </p>
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_uninstall_scanner</code> 
    </td>
    <td>
        Root / Local Administrator rights
    </td>
    <td>
        <code>localhost</code><br/>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        This playbook uninstalls the disconnected scanner from the targeted managed nodes from the following locations:
        <ul>
            <li><b>[UNIX/Linux]</b> <code>{{lmt_scanner_path_unix}}</code></li>
            <li><b>[Windows]</b> <code>{{lmt_scanner_path_windows}}</code></li>
        </ul>
        <b>NOTE:</b> This playbook removes the entire folder in which the disconnected scanner is installed.<br/><br/>
      Because it removes the internal endpoint ID, a new endpoint ID is generated if you install the scanner on the same computer. As a result, the computer is reported in License Metric Tool as a different computer than before.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_reconfigure_scanner</code> 
    </td>
    <td>
        Root / Local Administrator rights
    </td>
    <td>
        <code>localhost</code><br/>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        This playbook allows you to change the configuration of the existing disconnected scanners.     
        It can be useful, for example when you want to change the software scan schedule, or modify extra settings on Solaris (DSD mode) or Linux on System z. Before you run this playbook, update Ansible parameters accordingly.<br/>
        <b>NOTE:</b> Every time you run this playbook, the schedule of hardware and software scans as well as the schedule of packing scan results is updated.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_collect_status</code> 
    </td>
    <td>
        Read rights to the <code>config</code> and <code>iso-swid</code> disconnected scanner folders
    </td>
    <td>
        <code>localhost</code><br/>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        This playbook collects information about the version of scanners that are installed on your managed nodes and the last status of the disconnected scanner configuration. The collected data in stored in the <code>{{lmt_local_file_storage_path}}/scanner_statuses.csv</code> file
        on the control node.<br/><br/>
        The comma-separated values in the <code>scanner_statuses.csv</code> file are:
        <ul>
            <li><b>Ansible Host</b> - as defined in the Ansible inventory</li>
            <li><b>Scanner path</b> - the value of the configured parameter
                <ul>
                    <li><b>[UNIX/Linux]</b> <code>lmt_scanner_path_unix</code></li>
                    <li><b>[Windows]</b> <code>lmt_scanner_path_windows</code></li>
                </ul>
            </li>
            <li><b>Endpoint ID</b> - unique identifier of the disconnected scanner taken from the <code>endpoint_id.txt</code> file
                        or <code>Not available</code> in case when scanner is not installed.</li>
            <li><b>Scanner Version</b> - version of the installed scanner taken from <code>.swidtag</code> file or <code>Not installed</code> in 
                        case when scanner is not installed</li>
            <li>
                <b>Installation status</b> - The status of the disconnected scanner installation.
                 The status is updated every time the <code>lmt_install_or_upgrade_scanner</code> or <code>lmt_reconfigure_scanner</code> playbook is run. Possible values:
                <ul>
                    <li><code>Successful</code> - the last scanner installation or upgrade, or the last modification of configuration parameters was successful</li>
                    <li><code>Failed</code> - the last scanner installation or upgrade, or the last modification of configuration parameters failed</li>
                    <li><code>Unknown</code> - for scanners older than version 9.2.22 where the status information was not yet available</li>
                    <li><code>Not available</code> - the scanner is not installed</li>
                </ul>
            </li>           
        </ul>
        The file is recreated during each run of the playbook and contains information only about managed nodes that were targeted in the last run of the playbook.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_collect_troubleshooting_data</code> 
    </td>
    <td>
        Read rights to entire disconnected scanner folder and write rights to the <code>work</code> subdirectory
    </td>
    <td>
        <code>localhost</code><br/>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        This playbook collects data that can be used to investigate potential problems. It creates the following package on all targeted managed nodes on which the disconnected scanner is installed:
        <ul>
        <li><b>[UNIX/Linux]</b> <code>{{Ansible Host}}_{{Endpoint ID}}.tar.gz</code></li>
        <li><b>[Windows]</b> <code>{{Ansible Host}}_{{Endpoint ID}}.zip</code></li>
        </ul>
        The package is compressed by using the following method:<br/>
        <ul>
            <li><b>[UNIX/Linux]</b> <code>community.general.archive</code> Ansible module.</li>
            <li><b>[Windows]</b> <code>zip.exe</code> delivered together with disconnected scanner.</li>
        </ul>
        Next, it downloads the files and stores them in the
        <code>{{lmt_local_file_storage_path}}/troubleshooting_data/</code> directory on the control node.
        <br/><br/>
        The package contains the following items:<br/>
        <ul>
            <li><code>ansible_facts.txt</code> file that contains a list of relevant information about the system in the JSON format</li>
            <li>content of the following directories:
            <ul>
            <li><code>config</code></li>
            <li><code>logs</code></li>
            <li><code>iso-swid</code></li>
            <li><code>work</code></li>
            <li><code>output</code></li>
            </ul></li>
        </ul>
    </td>
  </tr>

</table>
