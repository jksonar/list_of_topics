
# 📘 Basic Ansible Concepts

### 1. What is Ansible and what is its primary purpose?

* **Definition:** Ansible is an open-source **IT automation tool** for configuration management, application deployment, and task automation.
* **Primary Purpose:** To make infrastructure setup and management **simple, agentless, and repeatable**.

Example:

```bash
ansible all -m ping
```

This checks connectivity with all hosts in your inventory.

---

### 2. Key features and advantages of Ansible

* **Agentless:** Uses SSH (no agent installation required).
* **Idempotent:** Ensures repeated runs don’t make unnecessary changes.
* **Declarative & Simple:** Written in YAML (easy to read/write).
* **Extensible:** Large library of modules for Linux, cloud, containers, etc.
* **Cross-platform:** Works on Linux, Windows, cloud services (AWS, Azure, GCP).

---

### 3. How does Ansible differ from Puppet or Chef?

* **Agentless vs Agent-based:** Puppet/Chef require agents on nodes, Ansible doesn’t.
* **Ease of use:** Ansible is YAML-based (simpler). Puppet/Chef use Ruby DSL.
* **Push vs Pull model:** Ansible pushes configs, Puppet/Chef mostly pull from master.
* **Learning curve:** Ansible is easier for beginners.

---

### 4. What is an Ansible Playbook and what is it used for?

* **Definition:** A YAML file that defines automation tasks to be executed on hosts.
* **Usage:** Run configurations, install packages, deploy applications.

Example `playbook.yml`:

```yaml
- name: Install Apache
  hosts: webservers
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present
```

---

### 5. Role of the Ansible Inventory file

* **Definition:** Lists target systems (hosts/groups).
* **Types:** Static (`hosts` file) or Dynamic (cloud APIs).

Example (`inventory.ini`):

```ini
[webservers]
192.168.1.10
192.168.1.11
```

---

### 6. What are Ansible Modules? Examples?

* **Modules:** Reusable units of code that perform specific tasks.
* **Examples:**

  * `yum` → install packages
  * `copy` → copy files
  * `service` → start/stop services
  * `user` → manage users

Ad-hoc example:

```bash
ansible webservers -m yum -a "name=httpd state=present"
```

---

### 7. What is Ansible Galaxy?

* A **repository of community-contributed roles**.
* Lets you share and download automation code.

Example:

```bash
ansible-galaxy install geerlingguy.apache
```

---

### 8. What are ad-hoc commands in Ansible?

* One-liner commands for quick tasks (no playbook needed).

Example:

```bash
ansible all -m shell -a "uptime"
```

---

### 9. Why does Ansible use YAML?

* **Human-readable** (easy for sysadmins).
* **Structured & lightweight.**
* **Widely supported.**

---

### 10. Concept of Idempotency in Ansible

* Means **running the same play multiple times produces the same result**.
* Example: `state=present` ensures a package is installed once, not multiple times.

---

# 📗 Intermediate Ansible Concepts

### 11. What are Ansible Roles? How do they differ from Playbooks?

* **Roles:** A structured way to organize playbooks into reusable components.
* **Playbook vs Role:**

  * Playbook = Defines tasks directly.
  * Role = Collection of tasks, vars, files, handlers, templates → reusable.

---

### 12. Explain Ansible Facts and how they are used

* **Facts:** System information gathered by Ansible (`ansible_facts`).
* Example:

```yaml
- debug:
    msg: "The hostname is {{ ansible_hostname }}"
```

---

### 13. How do you define and use variables in Ansible?

* **Defined in:** playbooks, inventory, `group_vars`, `host_vars`.
* Example:

```yaml
vars:
  package_name: httpd
tasks:
  - yum:
      name: "{{ package_name }}"
      state: present
```

---

### 14. What are Handlers in Ansible and when are they used?

* **Handlers:** Special tasks triggered only when notified.
* Use case: Restart service only if config changes.

Example:

```yaml
tasks:
  - copy:
      src: httpd.conf
      dest: /etc/httpd/conf/httpd.conf
    notify: Restart Apache

handlers:
  - name: Restart Apache
    service:
      name: httpd
      state: restarted
```

---

### 15. How to manage sensitive data in Ansible?

* **Ansible Vault:** Encrypt secrets (passwords, keys).

Example:

```bash
ansible-vault create secrets.yml
ansible-playbook site.yml --ask-vault-pass
```

---

### 16. Difference between static and dynamic inventory

* **Static:** Defined in `.ini` or `.yaml` files.
* **Dynamic:** Generated from cloud providers, scripts, or APIs (AWS, Azure).

---

### 17. Handling errors and failures in playbooks

* Use `ignore_errors: yes`.
* Use `failed_when` to define custom failure.
* Use `rescue` and `always` blocks.

---

### 18. What is Ansible Tower/AWX?

* **Tower:** Enterprise UI for Ansible (job scheduling, RBAC, reporting).
* **AWX:** Open-source upstream version.

---

### 19. How does Ansible integrate with CI/CD pipelines?

* Ansible playbooks can be triggered from Jenkins, GitLab CI/CD, GitHub Actions.
* Used for deployments, config updates after code build/test.

---

### 20. Purpose of tags in Ansible

* **Tags:** Run only specific parts of a playbook.

Example:

```yaml
tasks:
  - name: Install Apache
    yum:
      name: httpd
      state: present
    tags: apache
```

Run with:

```bash
ansible-playbook site.yml --tags "apache"
```

---

# 📙 Advanced & Scenario-Based Questions

### 21. Describe the Ansible architecture

* **Control Node:** Runs Ansible.
* **Managed Nodes:** Target systems.
* **Inventory:** List of hosts.
* **Playbooks & Modules:** Define automation.
* **Plugins:** Extend functionality (e.g., logging).

---

### 22. Managing different environments (dev, test, prod)

* Use **separate inventory files** and `group_vars`.
* Example:

```
inventories/dev/hosts.ini
inventories/prod/hosts.ini
```

---

### 23. Implementing rolling updates with zero downtime

* Use `serial` keyword in playbook.

Example:

```yaml
- hosts: webservers
  serial: 2
  tasks:
    - service:
        name: httpd
        state: restarted
```

---

### 24. Handling configuration drift

* Run playbooks regularly (cron, Tower schedules).
* Use `ansible-pull` for pull-based enforcement.
* Use CI/CD to enforce versioned configs.

---

### 25. Strategies for optimizing playbook performance

* Use `fact_caching`.
* Limit hosts with `--limit`.
* Use `async` & `poll`.
* Avoid unnecessary `gather_facts`.

---

### 26. Troubleshooting a failing playbook

* Use `-vvv` for debug logs.
* Check inventory, SSH connectivity.
* Run modules manually.
* Use `ansible-playbook --step` for step-by-step execution.

---

### 27. Use of Ansible Collections

* Package of roles, modules, and plugins.
* Example:

```bash
ansible-galaxy collection install community.general
```

---

### 28. Ansible & Infrastructure as Code (IaC)

* Defines infra in version-controlled YAML files.
* Integrates with Terraform for provisioning + Ansible for configuration.

---

### 29. Example of a complex project using Ansible

* **Scenario:** Automated deployment of a Django app.

  * Provision servers (Terraform).
  * Configure DB, webserver (Ansible).
  * Deploy app with `git pull`.
  * Manage secrets with Vault.
  * Rolling updates with zero downtime.

---

### 30. Ensuring security compliance & auditing

* Use **Ansible security roles** (e.g., CIS benchmarks).
* Enforce firewall, patch updates.
* Audit with Ansible Tower logs.
* Use `ansible-lint` for playbook security best practices.

---
Perfect 👍 You can automate this with **Ansible** by using the `command` or `openssl_certificate` module.
Since you want a **self-signed certificate non-interactively**, here are two approaches:

---

### ✅ Option 1: Using `command` module

Straight execution of your `openssl` command:

```yaml
- name: Generate self-signed SSL certificate
  hosts: all
  become: yes
  tasks:
    - name: Create SSL directory
      file:
        path: /etc/httpd/ssl
        state: directory
        mode: '0755'

    - name: Generate self-signed certificate with OpenSSL
      command: >
        openssl req -x509 -nodes -days 365
        -newkey rsa:2048
        -keyout /etc/httpd/ssl/{{ DomainName }}.key
        -out /etc/httpd/ssl/{{ DomainName }}.crt
        -subj "/C=IN/ST=Gujarat/L=Ahmedabad/O=MyCompany/OU=IT/CN={{ DomainName}}/emailAddress=admin@{{ DomainName }}"
      args:
        creates: /etc/httpd/ssl/{{ DomainName }}.crt
```

* `creates:` ensures the task runs only once (idempotency).
* `{{ DomainName }}` should be set in `vars:` or inventory.

---

### ✅ Option 2: Using Ansible’s `openssl_certificate` module

Cleaner and more Ansible-native:

```yaml
- name: Generate self-signed SSL certificate with Ansible module
  hosts: all
  become: yes
  tasks:
    - name: Create SSL directory
      file:
        path: /etc/httpd/ssl
        state: directory
        mode: '0755'

    - name: Generate self-signed certificate
      community.crypto.openssl_certificate:
        path: /etc/httpd/ssl/{{ DomainName }}.crt
        privatekey_path: /etc/httpd/ssl/{{ DomainName }}.key
        csr_path: /etc/httpd/ssl/{{ DomainName }}.csr
        provider: selfsigned
        subject:
          common_name: "{{ DomainName }}"
          country_name: "IN"
          state_or_province_name: "Gujarat"
          locality_name: "Ahmedabad"
          organization_name: "MyCompany"
          organizational_unit_name: "IT"
          email_address: "admin@{{ DomainName }}"
        days: 365
```

⚡ This requires the **`community.crypto`** collection:

```bash
ansible-galaxy collection install community.crypto
```

---

