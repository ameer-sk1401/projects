Here’s a README.md for your Ansible MySQL Database Installation & Configuration Project—including full implementation steps and manual verification:

⸻

# Ansible: MySQL Database Installation & Secure Configuration

## 📁 Project Structure

.
├── inventory/
│ └── hosts.ini
├── roles/
│ └── database/
│ ├── tasks/
│ │ └── main.yml
│ └── vars/
│ └── main.yml
└── database_configuration.yml

## 🚩 Project Requirement

    •	Install MySQL (or MariaDB) server on target host(s).
    •	Set up a secure root account (via socket auth or password).
    •	Create an application database and user with full privileges.
    •	Remove test database and anonymous users.

## 🛠️ How to Implement (Step-by-Step)

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd <your-repo-directory>



⸻

2. Configure the Inventory

Edit inventory/hosts.ini:

[database_servers]
34.236.67.80 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/ansible_rsa



⸻

3. Define Role Variables

roles/database/vars/main.yml:

app_db_name: myappdb
app_db_user: myappuser
app_db_password: SuperSecretPass
# mysql_root_password: Use if your MySQL root user uses password auth (see notes below)

Tip: Use Ansible Vault to encrypt secrets:

ansible-vault encrypt roles/database/vars/main.yml



⸻

4. Write Role Tasks

roles/database/tasks/main.yml:

- name: Install PyMySQL Python library (required for Ansible MySQL modules)
  ansible.builtin.apt:
    name: python3-pymysql
    state: present
  become: true

- name: Install MySQL server
  ansible.builtin.apt:
    name: mysql-server
    state: present
  become: true

- name: Ensure MySQL service is running
  ansible.builtin.service:
    name: mysql
    state: started
    enabled: true
  become: true

- name: Create application database
  community.mysql.mysql_db:
    name: "{{ app_db_name }}"
    state: present
    login_user: root
    login_unix_socket: /var/run/mysqld/mysqld.sock
  become: true

- name: Create application user
  community.mysql.mysql_user:
    name: "{{ app_db_user }}"
    password: "{{ app_db_password }}"
    priv: "{{ app_db_name }}.*:ALL"
    host: "%"
    state: present
    login_user: root
    login_unix_socket: /var/run/mysqld/mysqld.sock
  become: true

- name: Remove anonymous users
  community.mysql.mysql_user:
    name: ''
    host_all: true
    state: absent
    login_user: root
    login_unix_socket: /var/run/mysqld/mysqld.sock
  become: true

- name: Remove test database
  community.mysql.mysql_db:
    name: test
    state: absent
    login_user: root
    login_unix_socket: /var/run/mysqld/mysqld.sock
  become: true



⸻

5. Create the Playbook

database_configuration.yml:

---
- name: Install and Configure Database on server
  hosts: database_servers
  become: true
  roles:
    - role: database



⸻

6. Run the Playbook

ansible-playbook -i inventory/hosts.ini database_configuration.yml

If your vars are encrypted, add:

ansible-playbook -i inventory/hosts.ini database_configuration.yml --ask-vault-pass



⸻

✔️ Verification Steps

1. Check MySQL Service

sudo systemctl status mysql

	•	Should display active (running).

⸻

2. Log In as root (via socket) and List Databases/Users

sudo mysql
SHOW DATABASES;
SELECT user, host FROM mysql.user;

	•	Your app DB and user should appear.

⸻

3. Test Login as App User

mysql -u myappuser -p myappdb

	•	Enter your app password, should get a MySQL prompt.

⸻

4. Test Privileges

CREATE TABLE test_table (id INT PRIMARY KEY);
DROP TABLE test_table;



⸻

5. Confirm Test DB/Anonymous User Removal

SHOW DATABASES;
SELECT user, host FROM mysql.user WHERE user = '';

	•	There should be no test DB and no anonymous users.

⸻

💡 Extensions

- Automate daily database backups using `mysqldump` and cron.
- Add stricter MySQL security settings.
- Support PostgreSQL or MongoDB with a similar role.

📚 References

Ansible MySQL Collection: https://docs.ansible.com/ansible/latest/collections/community/mysql/
Securing MySQL: https://dev.mysql.com/doc/refman/8.0/en/security.html

---

Let me know if you need an **example backup task** or have issues with your automation!
```
