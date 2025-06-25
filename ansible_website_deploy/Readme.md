Ansible Website Deployment Project

This project uses Ansible to automate the deployment of a static website onto web servers. The site files (HTML, assets, etc.) are copied and managed using an Ansible role for easy reuse and scalability.

⸻

Project Structure

.
├── .ansible/
├── inventory/
│ └── hosts.ini
├── webserver/
│ ├── files/
│ │ ├── assets/
│ │ ├── vendor/
│ │ ├── contact.html
│ │ ├── index.html
│ │ ├── product-details.html
│ │ └── shop.html
│ ├── tasks/
│ │ └── main.yml
│ └── vars/
│ └── main.yml
└── ansible_website.yml

⸻

How It Works
• Role-based: The deployment logic is kept in a reusable webserver role.
• Idempotent: Running the playbook multiple times won’t break your server or duplicate files.
• Customizable: You can adjust variables like the service name, source, or destination directories.

⸻

Setup Instructions

1. Clone the Repository

git clone <your-repo-url>
cd <your-repo-directory>

2. Inventory Configuration

Edit inventory/hosts.ini to list your target web server(s).
For example:

[webserver]
192.0.2.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

3. Customize Website Content

Place your website files in webserver/files/ (replace or add HTML, images, etc.).

4. Set Variables (Optional)

Edit webserver/vars/main.yml if you want to change:
• The web service to install (service_name)
• The website source directory (source_directory)
• The destination directory on the server (destination_directory)

Default:

service_name: apache2
source_directory: files/
destination_directory: /var/www/html/

5. Run the Playbook

ansible-playbook -i inventory/hosts.ini ansible_website.yml

⸻

What the Playbook Does 1. Installs Apache2 (or your chosen service). 2. Starts and enables the web server. 3. Removes any existing content in the destination directory. 4. Creates the destination directory with proper permissions. 5. Copies all website files from webserver/files/ to the server’s web root.

⸻

Extending This Project
• Use Ansible Vault for secrets if needed.
• Switch service_name to nginx for Nginx support.
• Add SSL/TLS deployment tasks.
• Add firewall rules to open HTTP/HTTPS ports.

⸻

Troubleshooting
• Make sure your SSH keys and user permissions are correct in hosts.ini.
• The default destination /var/www/html/ must be writable by your Ansible user (or use become: true as in the playbook).

⸻

References
• Ansible Documentation
• Ansible Roles Best Practices

⸻

Happy Automation..!
