# Scheduled Backups

Scheduled Backups is a collection of Ansible playbooks to run automated backup tasks across different targets.  

Although the platforms differ in their specific backup procedures, the overall workflow can generally be broken down into three main steps: 
1. Create the required directory if it does not already exist
2. Trigger the backup and copy it to the local machine
3. Clean up previously created backups on the remote device

Playbooks are typically designed to implement the full backup workflow from creating the required directory, triggering and retrieving the files to clean up processes — these are considered fully implemented.  

### Fully implemented Platforms: 
- Cisco ESA (AsyncOS)
- Cisco Router and Swtiches (IOS / NXOS)
- F5 BigIP
- Linux Hosts

However, some playbooks only cover parts of the backup workflow — these are considered partly implemented, meaning additional steps must be configured or executed directly on the remote host.  

### Partly implemented Platforms:  
- Check Point Firewalls  
  *Backups are generated locally by a scheduled task on the firewall itself. The Check Point playbook is used only to copy the generated backups to a specified directory.*
- Any platform with a local scheduled backup function  
  *The local system generates and uploads backups to a remote repository. The Dummy Playbook is used to prepare the required directory structure. It must therefore be executed before the scheduled backup job runs on the host (e.g. Cisco ISE).*

## Installation

```bash
# Install dependencies
apt install curl sshpass python3-venv python3-pip libssh-dev ansible

# Clone the repository
git clone https://github.com/l3fuex/scheduled-backups.git
```

## Scheduling
Add the following line to your crontab with `crontab -e` to schedule backups for 2AM.
```bash
0 2 * * *  ansible-playbook /PATH/TO/main.yaml --inventory /PATH/TO/inventory
```

## Notes
- Review and adjust the inventory file and playbook variables as needed.
- ssh access needs to be properly configured for all target systems.
- There is a [bug](https://github.com/ansible-collections/ansible.netcommon/issues/698) in Ansible 2.19 related with network_cli. Workaround: install an older Ansible version such as 2.18.10
