# Ansible: Package Update & Security Patch Automation

## 📁 Project Structure

        .
        ├── inventory/
        │   └── hosts.ini
        ├── update_pkg/
        │   ├── tasks/
        │   │   └── main.yml
        └── update_pkg.yml

## 🚩 Project Requirement

	•	Update all system packages to their latest versions on one or more servers.
	•	Apply security patches (if available).
	•	Optionally, reboot the server only if a kernel or critical package update occurred.

## 🛠️ How to Implement (Step-by-Step)

### 1. Clone the Repository


        git clone <your-repo-url>
        cd <your-repo-directory>

⸻

2. Configure the Inventory

Edit inventory/hosts.ini with your target server(s):

        [all]
        192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa


⸻

3. Run the Playbook

ansible-playbook -i inventory/hosts.ini update_pkg.yml


⸻

💡 Extensions

- Add email or Slack notifications after updates and/or reboots.
- Use a cron job or CI/CD runner to schedule patching.
- Add package exclusion lists or only update security-related packages.

📝 Notes

- This playbook supports both Debian/Ubuntu and RedHat/CentOS systems.
- Servers may reboot if kernel or critical updates require it.
- Always test first in a non-production environment.

📚 References

Ansible Documentation: https://docs.ansible.com/
Best Practices for Ansible Roles: https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
