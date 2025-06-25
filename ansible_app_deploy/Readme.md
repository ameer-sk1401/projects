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

	git clone <your-repo-url>
	cd <your-repo-directory>

⸻

2. Configure the Inventory

Edit inventory/hosts.ini:

	[appserver]
	192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa


⸻


3. Run the Playbook

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


