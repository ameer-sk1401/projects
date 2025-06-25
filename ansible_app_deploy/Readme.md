Here’s Project 4: Deploy a Sample Application
This is written in Markdown and shows how to go from clone to execution, including directory structure, exact files, and commands.

⸻

# Ansible: Sample Application Deployment (Flask Example)

## 📁 Project Structure

.
├── inventory/
│ └── hosts.ini
├── flask_app/
│ ├── files/
│ │ └── app.py
│ ├── tasks/
│ │ └── main.yml
│ └── templates/
│ └── flaskapp.service.j2
└── deploy_flask.yml

## 🚩 Project Requirement

    •	Use Ansible to deploy a simple Python Flask application on a remote server.
    •	Ensure Python and pip are installed.
    •	Install required Python packages.
    •	Set up a systemd service to run the Flask app.
    •	Copy application code to the server.
    •	Start and enable the Flask app service.

## 🛠️ How to Implement (Step-by-Step)

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd <your-repo-directory>



⸻

2. Configure the Inventory

Edit inventory/hosts.ini:

[appserver]
192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa



⸻

3. Place Application Files
	•	Put your app.py (a basic Flask app) in flask_app/files/.

Example app.py:

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask deployed by Ansible!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)



⸻

4. Create a Systemd Template

flask_app/templates/flaskapp.service.j2

[Unit]
Description=Gunicorn instance to serve Flask App
After=network.target

[Service]
User=ubuntu
Group=ubuntu
WorkingDirectory=/opt/flask_app
ExecStart=/usr/bin/python3 /opt/flask_app/app.py

[Install]
WantedBy=multi-user.target



⸻

5. Write Role Tasks

flask_app/tasks/main.yml

- name: Ensure Python3 and pip are installed
  ansible.builtin.apt:
    name:
      - python3
      - python3-pip
    state: present
  become: true

- name: Install Flask using pip
  ansible.builtin.pip:
    name: flask
    executable: pip3

- name: Ensure application directory exists
  ansible.builtin.file:
    path: /opt/flask_app
    state: directory
    owner: ubuntu
    group: ubuntu

- name: Copy application code to server
  ansible.builtin.copy:
    src: app.py
    dest: /opt/flask_app/app.py
    owner: ubuntu
    group: ubuntu
    mode: '0755'

- name: Set up systemd service for flask app
  ansible.builtin.template:
    src: flaskapp.service.j2
    dest: /etc/systemd/system/flaskapp.service
  become: true

- name: Reload systemd
  ansible.builtin.systemd:
    daemon_reload: yes
  become: true

- name: Start and enable flaskapp service
  ansible.builtin.systemd:
    name: flaskapp
    state: started
    enabled: yes
  become: true



⸻

6. Create the Playbook

deploy_flask.yml

---
- name: Deploy Flask Application
  hosts: appserver
  become: true
  roles:
    - role: flask_app



⸻

7. Run the Playbook

ansible-playbook -i inventory/hosts.ini deploy_flask.yml



⸻

💡 Extensions

- Deploy other applications (Node.js, PHP, etc.) by modifying the role.
- Pull application code from a Git repository.
- Add Nginx as a reverse proxy for the Flask app.
- Use Ansible Vault for app secrets or environment variables.

📝 Notes

- Ensure your target server’s firewall allows inbound traffic to port 5000 (or change the Flask app port as needed).
- For production, consider running Flask with Gunicorn or behind Nginx.

📚 References

Flask Documentation: https://flask.palletsprojects.com/
Ansible Documentation: https://docs.ansible.com/

---

Let me know when you’re ready for the next project, or if you want step-by-step for a **different stack** (Node.js, Java, etc.)!
```
