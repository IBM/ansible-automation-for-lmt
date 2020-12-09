# License Metric Tool parameters

<table>

  <tr>
    <th rowspan="2">Parameter</th>
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
    <td><code>{{lmt_scanner_path_unix}}/output</code></td>
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
    <td><code>{{lmt_scanner_path_windows}}\output</code></td>
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
    <td>"yes"</td>
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
		Possible values: "yes" or "no" (with quotes).
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
        Remember to set all four parameters in such case othervise the scanner installation or configuration may hang.
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
		<code>lmt_scanner_software_scans_enabled</code> 
	</td>
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
		<code>lmt_scanner_daily_pack_results_enabled</code> 
	</td>
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

</table>