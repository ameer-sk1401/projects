# Ansible Static Website Deployment

This project demonstrates how to use **Ansible** to automate the deployment of a static website to a Linux web server using a simple, reusable role structure.

---

## 📁 Project Structure

.
├── .ansible/
├── inventory/
│   └── hosts.ini
├── webserver/
│   ├── files/
│   │   ├── assets/
│   │   ├── vendor/
│   │   ├── contact.html
│   │   ├── index.html
│   │   ├── product-details.html
│   │   └── shop.html
│   ├── tasks/
│   │   └── main.yml
│   └── vars/
│       └── main.yml
└── ansible_website.yml

---

## 🚀 What Does This Do?

- Installs the web server (`apache2` by default, configurable)
- Removes existing content in the web root (default: `/var/www/html/`)
- Copies all static website files (HTML, assets, etc.) from `webserver/files/` to the web root
- Ensures the web service is started and enabled on boot

---

## ⚙️ Setup & Usage

### 1. **Clone the repository**

```bash
git clone <your-repo-url>
cd <your-repo-directory>

2. Edit Your Inventory

List your target hosts in inventory/hosts.ini.
Example:

[webserver]
192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

3. Customize Website Content
	•	Place your static files (HTML, images, etc.) in webserver/files/.

4. Configure Variables (Optional)

Edit webserver/vars/main.yml to change:

service_name: apache2                # or nginx
source_directory: files/
destination_directory: /var/www/html/

5. Run the Playbook

ansible-playbook -i inventory/hosts.ini ansible_website.yml



⸻

📝 Playbook Overview

The role in webserver/tasks/main.yml will:
	1.	Ensure the specified service is installed (service_name)
	2.	Start and enable the service
	3.	Remove the destination directory and its contents
	4.	Recreate the directory with correct permissions
	5.	Copy your website files to the server

⸻

🔧 Extending This Project
	•	Switch service_name to nginx for Nginx support
	•	Add firewall rules (UFW/firewalld) to open HTTP/HTTPS ports
	•	Use Ansible Vault for secrets (if needed)
	•	Add SSL/HTTPS deployment tasks
	•	Integrate with CI/CD for automated deployments

⸻

🐞 Troubleshooting
	•	Ensure your ansible_user and SSH key in hosts.ini are correct
	•	Target hosts should allow SSH and have Python installed
	•	Run with -vvv for detailed logs if something goes wrong

⸻

📚 References
	•	Ansible Documentation
	•	Best Practices for Ansible Roles

⸻

Happy automating!

