# Parameters used in playbooks

Before you run License Metric Tool playbooks, review default values of License Metric Tool parameters and adjust them according to your needs.


<table>

  <tr>
    <th rowspan="2">Parameter</th>
	<th>Available from</th>
    <th>Default value</th>
    <th>Applicable nodes</th>
    <th>Playbooks that use the parameter</th>   
  </tr>
  <tr>    
	<th colspan="4">Description</th>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_local_file_storage_path</code> 
	</td>
	<td>9.2.22</td>
    <td>
		<code>./lmt_file_storage</code> <br/>
		It is a subdirectory of the directory where the playbooks are located.
	</td>
    <td>All managed nodes</td>
	<td>
		<code>collect_results</code><br/>
		<code>install_or_upgrade</code><br/>
		<code>collect_status</code><br/>
		<code>troubleshooting</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path on a control node where the License Metric Tool files are stored. It contains packages with disconnected scan results, troubleshooting data, installed scanner and status data retrieved from all computers.<br/>
		Do not place the directory in a temporary location (for example, <code>/tmp</code>) as this is expected to be a persistent storage of License Metric Tool files. 
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_server_path</code> 
	</td>
	<td>9.2.22</td>
    <td>
		<b>[Linux]</b> <code>/opt/ibm/LMT</code><br/>
		<b>[Windows]</b> <code>{{ansible_env['ProgramFiles']}}</code><br/><code>\ibm\LMT</code>
	</td>
    <td><code>lmt_server</code></td>
	<td>
		<code>collect_results</code><br/>
		<code>install_or_upgrade</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path to the main directory of the License Metric Tool server on the <code>lmt_server</code> node.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_server_datasource_path</code> 
	</td>
	<td>9.2.22</td>
    <td>
		<b>[Linux]</b> <code>{{lmt_server_path}}/datasource</code><br/>
		<b>[Windows]</b> <code>{{lmt_server_path}}\\datasource</code>
	</td>
    <td><code>lmt_server</code></td>
	<td>
		<code>collect_results</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path to a disconnected data source directory on the License Metric Tool server.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2">
		<code>lmt_scanner_path_unix</code>
	</td>
	<td>9.2.22</td>
    <td><code>/var/opt/ibm/LMTScanner</code></td>
    <td>Managed nodes on UNIX/Linux</td>
	<td>
		<code>collect_results</code><br/>
		<code>install_or_upgrade</code><br/>
		<code>collect_status</code><br/>
		<code>troubleshooting</code><br/>
		<code>uninstall</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path on the UNIX and Linux managed nodes where the scanner will be installed.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_output_path_unix</code>
	</td>
	<td>9.2.22</td>
    <td>
        <code>{{lmt_scanner_path_unix}}</code><br/>
        &nbsp;
        <code>/output</code>
    </td>
    <td>Managed nodes on UNIX/Linux</td>
	<td>
		<code>collect_results</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path to a directory where packages with scan results are generated on UNIX/Linux.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_path_windows</code> 
	</td>
	<td>9.2.22</td>
    <td>
        <code>{{ansible_env['ProgramFiles']}}</code><br>
        &nbsp;
        <code>\ibm\LMTScanner</code>
    </td>
    <td>Managed nodes on Windows</td>
	<td>
		<code>collect_results</code><br/>
		<code>install_or_upgrade</code><br/>
		<code>collect_status</code><br/>
		<code>troubleshooting</code><br/>
		<code>uninstall</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path on the Windows managed nodes where the scanner will be installed.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_output_path_windows</code>
	</td>
	<td>9.2.22</td>
    <td>
        <code>{{lmt_scanner_path_windows}}</code><br/>
        &nbsp;
        <code>\output</code>
    </td>
    <td>Managed nodes on Windows</td>
	<td>
		<code>collect_results</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Path to a directory where packages with scan results are generated on Windows.
		<br/><br/>
		Possible values: Path (string)
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_setup_timeout</code> 
	</td>
	<td>9.2.22</td>
    <td>300</td>
    <td>All managed nodes</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Number of seconds after which the scanner setup and uninstaller is terminated and reported as failed if it does not finish within the defined time.
		<br/><br/>
		Possible values: Number
	</td>
  </tr>

  <tr>
    <td rowspan="2">
		<code>lmt_scanner_solaris_dsd_mode</code> 
	</td>
	<td>9.2.22</td>
    <td>false</td>
    <td>Managed nodes on Solaris</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Applicable only for Solaris. Information whether the Solaris system is in the DSD domain. 
		<br/><br/>
		Possible values: true/false.
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
        All starting with <code>lmt_scanner_zlinux</code>:<br/>
		<code>machine_type</code><br/>
		<code>processor_type</code><br/>
		<code>shared_pool_capacity</code><br/>
		<code>system_active_processors</code><br/> 
	</td>
	<td>9.2.22</td>
    <td>Not set. It assumes that automatic capacity configuration is supported. Otherwise, installation or reconfiguration fails.</td>
    <td>Managed nodes on Linux on System z</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		A set of parameters that describe underlying hardware of the System z host on which the scanner is going to be installed. The parameters need to be defined if the automatic capacity configuration is not supported.
        Remember to set all four parameters in such case otherwise the scanner installation or configuration may hang.
		<br/><br/>
		Automatic capacity configuration is supported on Linux on z/KVM and on System z10 starting from model E64 (type 2097) mainframes with z/VM 6.3 and later that supports the Store Hypervisor Information (STHYI) instruction.<br/>
		Additionally, global performance data control option must be set in HMC for LPARs for the automatic capacity configuration to work.
		<br/><br/>
		Example:<br>
		On a z9 machine with 12 IFL processors running on a shared pool with the capacity of 20, set the variables as follows:<br/>
		<code>lmt_scanner_zlinux_machine_type: "z9"</code><br/>
		<code>lmt_scanner_zlinux_processor_type: "IFL"</code><br/>
		<code>lmt_scanner_zlinux_shared_pool_capacity: "20"</code><br/>
		<code>lmt_scanner_zlinux_system_active_processors: "12"</code>
		<br/><br/>
		Possible values: Strings
	</td>
  </tr>

<tr>
    <td rowspan="2"> 
		<code>lmt_scanner_software_scan_cpu_threshold_percentage</code> 
	</td>
	<td>9.2.27</td>
    <td>Empty (none)</td>
    <td>All managed nodes</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Limits the amount of processor resources that the scanner consumes. By default, the value is empty which indicates that the scan can consume up to 100% of a single CPU that is available to the scanner. The higher value you specify as the threshold, the higher is the consumption limit. For example, if you specify 75, scanner processes use the average of 75% of a single CPU that is available on the target computer.
    <br/><br/>
     <b>Important:</b> Setting the threshold does not guarantee that CPU consumption is always below the specified value. It fluctuates around that value, sometimes exceeding it and sometimes dropping below it. Temporary peaks are expected. Setting the threshold might lengthen the time of the scan. 
		<br/><br/>
		Possible values: Empty, or integer between 5 - 100
	  </td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_software_scans_enabled</code> 
	</td>
	<td>9.2.22</td>
    <td>true</td>
    <td>All managed nodes</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Enables scheduling software scans in cron. If you set the value
		of this parameter to <code>true</code>, the first software scan is initiated after you setup the scanner. Subsequent scans run with the frequency that is set in the <code>lmt_scanner_software_scans_frequency</code> parameter.
		After each execution of the software scan, packing results is automatically triggered.
		<br/><br/>
		Possible values: true/false.
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_software_scans_frequency</code> 
	</td>
	<td>9.2.22</td>
    <td>WEEKLY</td>
    <td>All managed nodes</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Defines the frequency of software scans when the scheduled software scans are enabled by the <code>lmt_scanner_software_scans_enabled</code> parameter.
		<br/><br/>
		Possible values: DAILY/WEEKLY.
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_software_scan_day_of_week</code> 
    </td>
    <td>9.2.26</td>
    <td>Empty (none)</td>
    <td>All managed nodes</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Defines the day of the week on which the weekly scan will run. This parameter is relevant only if the <code>lmt_scanner_software_scans_frequency</code> parameter is set to WEEKLY. By default, this parameter is empty which means that the weekly scan will be scheduled starting from the time when the <code>install or upgrade</code> or <code>reconfigure</code> playbook was run.
        <br/><br/>
        Possible values: EMPTY/MON/TUE/WED/THU/FRI/SAT/SUN.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_software_scan_local_time</code> 
    </td>
    <td>9.2.26</td>
    <td>Empty (none)</td>
    <td>All managed nodes</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Defines the time (hour and minutes) during the day in the LOCAL time zone of manage node when the scheduled software scan (weekly or daily) will run. By default, this parameter is empty which means that the scan will start at the time of the day when the <code>install or upgrade</code> or <code>reconfigure</code> playbook was run.
        <br/><br/>
        Example:<br/>
        <code>lmt_scanner_software_scan_local_time: "20:30"</code><br/>
        <b>Note:</b> Ensure the time string is enclosed in quotation marks.
        <br/><br/>
        Possible values: EMPTY or time in the "HH:MM" format.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
		<code>lmt_scanner_daily_pack_results_enabled</code> 
	</td>
	<td>9.2.22</td>
    <td>true</td>
    <td>All managed nodes</td>
	<td>
		<code>install_or_upgrade</code><br/>
		<code>reconfigure</code>
	</td>
  </tr>
  <tr>    
    <td colspan="4">
		Enables scheduling daily creation of the scan results package. If you set the value of this parameter to <code>true</code>, creation of the first scan results package is initiated 23 hours after you install the scanner. Subsequent packages are generated daily.
		<br/><br/>
		Possible values: true/false.
	</td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_public_cloud_type</code> 
    </td>
	<td>9.2.23</td>
    <td>Empty (none)</td>
    <td>All managed nodes</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Specifies the type of a public cloud on which the computer is running. It allows for properly counting the number of Processor Value Units (PVUs) per virtual core.
        <br/><br/>
        Example:<br/>
        <code>lmt_scanner_public_cloud_type: "Microsoft Azure"</code><br/>
        <b>Note:</b> Ensure the name of the public cloud is enclosed in quotation marks.
        <br/><br/>
        Possible values include names of supported public clouds such as:<br>
        <code>"IBM Cloud Virtual Server"</code><br>
        <code>"IBM Power Virtual Server"</code><br>
        <code>"IBM SoftLayer"</code><br>
        <code>"IBM Cloud LinuxONE VS"</code><br>
        <code>"Microsoft Azure"</code><br>
        <code>"Amazon EC2"</code><br>
        <code>"Google Compute Engine"</code><br>
        <code>"Oracle Compute Instance"</code><br>
        <code>"Alibaba Elastic Compute Service"</code><br>
        <code>"Tencent Cloud Server Instance"</code><br>
        <code>"NEC Cloud IaaS Instance"</code><br>
        <code>"Fujitsu Cloud IaaS Instance"</code><br>
        <code>"NTT Enterprise Cloud Server"</code><br>
        <code>"NTT IaaS Powered by VMware"</code><br>
        <code>"NTT Data"</code><br>
        <code>"KDDI Virtual Server"</code><br>
        <br/>
        For more information about supported types of public clouds, see:  <a href="https://ibm.biz/LMT_public_clouds_disconnected">Identifying disconnected computers as running on public clouds</a> in License Metric Tool documentation.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_virt_host_scan_enabled</code> 
    </td>
    <td>9.2.24</td>
    <td>false</td>
    <td>Managed nodes on Linux</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Enables capacity scan on virtualization host to retrieve capacity data from all virtual machines that are managed by the specified host. 
        Schedule the scan on every host that manages virtual machines which have the disconnected scanner installed.
        <br/><br/>
        Supported virtualization technologies:<br/>
        <ul>
          <li>Xen on Linux x86</li>
          <li>KVM on Linux x86</li>
          <li>KVM on Power Linux</li>
        </ul>
        <br/>
        Prerequisites on the KVM host:<br/>
        <ul>
          <li>Operating system: Linux x86 or ppc64</li>
          <li>Libvirt-client library installed on the host (virsh command available) or xl command available</li>
          <li>Libxml2 library installed on the host (xmllint command available)</li>
          <li>Bash shell available</li>
        </ul>
        <br/>
        Prerequisites on the Xen host:<br/>
        It is recommended to collect capacity from Xen data by using the VM Manager Tool instead of this task. For more information, see the product documentation.
        <br/><br/>
        Possible values: true/false.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_collect_host_hostname</code> 
    </td>
    <td>9.2.24</td>
    <td>false</td>
    <td>Managed nodes on Linux</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Enable this option if you want to collect information about host names of virtualization hosts during virtualization host scan (the lmt_scanner_virt_host_scan_enabled parameter needs to be enabled).
        <br/><br/>
        Possible values: true/false.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_scanner_docker_scan_enabled</code> 
    </td>
    <td>9.2.26</td>
    <td>false</td>
    <td>Managed nodes on Linux</td>
    <td>
        <code>install_or_upgrade</code><br/>
        <code>reconfigure</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Set the value of this parameter to TRUE to enable the scan of all Docker containers that are deployed on the target managed node.
        <br/><br/>
        Possible values: true/false.
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_server_token</code> 
    </td>
    <td>9.2.24</td>
    <td>Empty (none)</td>
    <td>All managed nodes</td>
    <td>
        <code>uninstall</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        API token of the License Metric Tool server. If the token is defined, the disconnected computer is automatically decommissioned from the License Metric Tool server during scanner uninstallation.
        <br/><br/>
        Possible values: String
    </td>
  </tr>

  <tr>
    <td rowspan="2"> 
        <code>lmt_server_port</code> 
    </td>
    <td>9.2.24</td>
    <td>9081</td>
    <td>All managed nodes</td>
    <td>
        <code>uninstall</code>
    </td>
  </tr>
  <tr>    
    <td colspan="4">
        Port number of the License Metric Tool server. It is used to connect to the License Metric Tool server API while decommissioning computers.
        <br/><br/>
        Possible values: Number
    </td>
  </tr>
</table>
