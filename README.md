# Patching Role

This Ansible role is designed to patch and reboot hosts, while updating statuses in a Google Sheet and handling Nagios monitoring.

### Supported Operating Systems
- Red Hat Enterprise Linux (RHEL)
- Ubuntu
- Windows Server

## Requirements

This role requires the following Ansible collections beyond the default `ansible.builtin`:

- `ansible.windows`: This collection is necessary for managing Windows updates and reboots.
- `theforeman.foreman`: This collection is necessary for managing RHEL updates and reboots through Red Hat Satellite.
- `community.general`: This collection is used for additional utilities such as JSON query and index lookup.


You can install these collections using the following command:

```bash
ansible-galaxy install -r requirements.yml
```

## Role Variables

All variables can be overridden in your playbook or inventory.

### Generic Variables
- `enforce_using_limit`: Enforce using `--limit` option
- `linux_ssh_port`: SSH port

### Red Hat Variables
- `redhat_patch_by`: you can select between using Hammer, Satellite Job Invoction or YUM (satellite-hammer | satellite-job | yum)
- `redhat_satellite_server`: Red Hat Satellite server FQDN
- `redhat_satellite_user`: User to connect to Red Hat Satellite
- `redhat_satellite_password`: Password of the user to connect to Red Hat Satellite
- `redhat_satellite_organization_name`: Red Hat Satellite organization name
- `redhat_satellite_job_search_query`: Query to match the ansible inventory host

### Google Sheet Variables
- `google_sheet_api_url`: API URL for Google Sheets.
- `google_sheet_id`: ID of the Google Sheet.
- `google_sheet_name`: Name of the sheet within the Google Sheet.
- `google_sheet_range`: Range of cells to read.
- `google_sheet_status_column`: Column to update status.
- `google_client_id`: Google Client ID.
- `google_client_secret`: Google Client Secret.
- `google_refresh_token`: Google Refresh Token.
To obtain the google_client_id, google_client_secret, and google_refresh_token, you need to create a new project in the Google Developers Console, enable the Google Sheets API, and create OAuth 2.0 credentials. For detailed instructions, refer to the Google Sheets API documentation.

### Nagios Variables
- `nagios_url`: URL for Nagios.
- `nagios_apikey`: API key for Nagios.
- `nagios_downtime_in_minutes`: Duration of scheduled downtime in minutes.
- `nagios_downtime_comment`: Comment for scheduled downtime.
- `nagios_schedule_downtime`: Boolean to schedule downtime.
To obtain the nagios_apikey, you need to generate an API key in your Nagios configuration. Refer to the Nagios documentation for instructions on how to generate an API key.

### Windows Variables
- `windows_winrm_port`: Windows WinRM port to connect to the Windows servers
- `windows_update_server_selection`: Source of Windows update either Windows Online Update catalog or WSUS (windows_update | managed_server )
- `windows_update_log_file`: Path to a log file to append the update progress
- `windows_update_reject_list`: List of Windows updates to reject.

Example Playbook
----------------

```yaml
- name: Patch All Host(s)
  hosts: all
  roles:
    - moemilan.os_patching
  vars:
    enforce_using_limit: "yes"
    linux_ssh_port: "22"
    redhat_patch_by: satellite-hammer
    redhat_satellite_server: "satellite.example.com"
    redhat_satellite_user: "satellite-user"
    redhat_satellite_password: "Password"
    redhat_satellite_organization_name: "Example"
    redhat_satellite_job_search_query: "name={{ inventory_hostname }}"
    google_sheet_id: "your_google_sheet_id"
    google_sheet_name: "your_google_sheet_name"
    google_sheet_range: "A:D"
    google_sheet_status_column: "C"
    google_client_id: "your_google_client_id"
    google_client_secret: "your_google_client_secret"
    google_refresh_token: "your_google_refresh_token"
    nagios_url: "your_nagios_url"
    nagios_apikey: "your_nagios_apikey"
    nagios_schedule_downtime: true
    windows_winrm_port: "5986"
    windows_update_server_selection: windows_update
    windows_update_log_file: c:\ansible_winupdate.txt
    windows_update_reject_list:
      - "Update for Microsoft Defender for Endpoint"
      - "Security Intelligence Update for Microsoft Defender Antivirus"
      - "Windows Malicious Software Removal Tool x64"
```

License
-------

Apache-2.0

Author Information
------------------

- **Name:** Mohamed Ahmed
- **Email:** [moemilan@gmail.com](mailto:moemilan@gmail.com)
- **GitHub:** [https://github.com/moemilan](https://github.com/moemilan)