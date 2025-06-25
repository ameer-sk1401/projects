# Ansible: Centralized Log Directory Setup & Log Rotation

## 📁 Project Structure

	.
	├── inventory/
	│ └── hosts.ini
	├── logsetup/
	│ ├── files/
	│ │ └── myapp.logrotate
	│ ├── tasks/
	│ │ └── main.yml
	└── logsetup.yml

## 🚩 Project Requirement

    •	Ensure every server has a /var/log/myapp/ directory with correct permissions.
    •	Deploy a logrotate configuration for this directory.

## 🛠️ How to Implement (Step-by-Step)

### 1. Clone the Repository

	git clone <your-repo-url>
	cd <your-repo-directory>

⸻

2. Configure the Inventory

Edit inventory/hosts.ini:

	[logserver]
	192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

⸻



3. Run the Playbook

ansible-playbook -i inventory/hosts.ini logsetup.yml



⸻

✔️ Verification Steps

	1. Check Directory and Permissions
	
	ls -ld /var/log/myapp
	
		•	Should show: drwxr-xr-x 2 root adm ... /var/log/myapp
	
	2. Check Logrotate Config
	
	cat /etc/logrotate.d/myapp
	
		•	Should show your config contents.
	
	3. Test Logrotate (Dry Run)
	
	sudo logrotate -d /etc/logrotate.d/myapp
	
		•	Shows what logrotate would do (no changes made).
	
	4. Force Logrotate (Real Run)
	
	sudo logrotate -f /etc/logrotate.d/myapp
	
		•	Rotates logs immediately (if any exist).

⸻

💡 Extensions

	- Use Jinja2 templates for dynamic configs.
	- Rotate logs for multiple applications.
	- Add cron jobs for custom log cleanup.

📚 References

	Logrotate Manual: https://linux.die.net/man/8/logrotate
	Ansible Documentation: https://docs.ansible.com/

