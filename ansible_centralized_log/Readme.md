Here’s a README.md for your Centralized Log Directory Setup & Log Rotation Ansible project, in clean Markdown with clear, step-by-step verification at the end:

⸻

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

```bash
git clone <your-repo-url>
cd <your-repo-directory>



⸻

2. Configure the Inventory

Edit inventory/hosts.ini:

[logserver]
192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa



⸻

3. Prepare Logrotate Config File

Create logsetup/files/myapp.logrotate:

/var/log/myapp/*.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
    create 0640 root adm
    sharedscripts
    postrotate
        systemctl reload myapp || true
    endscript
}



⸻

4. Write Role Tasks

logsetup/tasks/main.yml:

- name: Ensure /var/log/myapp exists
  ansible.builtin.file:
    path: /var/log/myapp
    state: directory
    owner: root
    group: adm
    mode: '0755'

- name: Ensure logrotate is installed
  ansible.builtin.package:
    name: logrotate
    state: present

- name: Deploy logrotate config for myapp
  ansible.builtin.copy:
    src: myapp.logrotate
    dest: /etc/logrotate.d/myapp
    owner: root
    group: root
    mode: '0644'



⸻

5. Create the Playbook

logsetup.yml:

---
- name: Centralized Log Directory & Log Rotation Setup
  hosts: logserver
  become: true
  roles:
    - role: logsetup



⸻

6. Run the Playbook

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

---

Let me know if you want to add troubleshooting tips, example output, or other automation details!
```
