# Release notes

The following table provides information about defects that were fixed in Ansible playbooks that are delivered with License Metric Tool.

<table frame="topbot" id="table_dwh_bg5_y4b">
<th colname="col1">Defect</th>
<th colname="col2">Symptoms</th>
<th colname="col3">Behavior after you install the update</th>
<th colname="col4">Available since</th>
</tr><tr>
<td>199336</td>
<td>Playbook for uninstalling the disconnected scanner fails if it is used when the scans are running.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.24</td>
</tr>
<tr>
<td>198832</td>
<td>If the disconnected scanner output directory is set to a non-default location, the Ansible
playbook for collecting troubleshooting data does not collect data from that directory.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198761</td>
<td>The disconnected scanner that is installed on Windows runs the software scan twice after
setup and produces two result packages that are almost identical.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198728</td>
<td>Ansible playbook for upgrading the disconnected scanner finishes successfully on Windows even
though the scanner is not upgraded. </td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198710</td>
<td>Ansible playbook for reconfiguring the disconnected scanner fails.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198706</td>
<td>Changing the values of the <parmname>lmt_scanner_output_path_unix</parmname> and
<parmname>lmt_scanner_output_path_windows</parmname> parameters in the Ansible inventory file takes
no effect. The scanner output is still generated in the default directory.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198641</td>
<td>When you enable daily creation of packages with disconnected scan results and then change
this schedule and run the Ansible playbook for reconfiguring the disconnected scanner, the change
does not take effect.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
<tr>
<td>198508</td>
<td>Ansible playbook for uploading scan results to the <keyword
conref="../conrefs.dita#shared/name_lmt_short"/> server fails when a single file with disconnected
scan results has to be uploaded to a <keyword conref="../conrefs.dita#shared/name_lmt_short"/>
server that is installed on Windows.</td>
<td>The problem does not occur.</td>
<td align="center">9.2.23</td>
</tr>
</tbody>
</table>