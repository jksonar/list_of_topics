# **Red Hat Satellite** often comes up in interviews, especially for **Linux admin / DevOps / sysadmin** roles where large-scale RHEL environments are managed.

---

# 🔹 What is Red Hat Satellite?

👉 **Red Hat Satellite** is a **system management platform** from Red Hat used to deploy, configure, and maintain **Red Hat Enterprise Linux (RHEL) systems at scale**.

Think of it as a **centralized management tool** for hundreds or thousands of RHEL servers.

It automates:

* **Provisioning** (installing OS & configuring servers)
* **Configuration management** (using Puppet/Ansible)
* **Patch management** (security updates & bug fixes)
* **Subscription management** (managing Red Hat licenses)
* **Monitoring & reporting**

---

## 🔹 Core Components of Red Hat Satellite

1. **Content Management**

   * Sync RHEL packages & updates from Red Hat CDN.
   * Mirror repositories (yum, rpm).
   * Distribute software content to servers.

2. **Provisioning**

   * Kickstart bare-metal servers or VMs with RHEL.
   * Integration with VMware, AWS, OpenStack, bare-metal PXE boot.
   * Automated host registration.

3. **Configuration Management**

   * Uses **Ansible** (earlier Puppet) to enforce configuration.
   * Apply consistent policies across servers.

4. **Patch Management**

   * Roll out **security & bug fixes**.
   * Stage updates in **Lifecycle Environments** (Development → QA → Production).

5. **Subscription & License Management**

   * Track and allocate Red Hat subscriptions across servers.
   * Prevent “out of subscription” issues.

6. **Monitoring & Reporting**

   * Compliance scanning.
   * Reports on patches, security, and performance.

---

## 🔹 Architecture Overview

* **Satellite Server** → Central management console (web UI & CLI).
* **Capsule Server** → Proxy server to manage remote sites/datacenters (acts like a content and provisioning relay).
* **Hosts (Clients)** → RHEL systems registered to Satellite.

👉 Analogy: **Satellite = Control Tower**, **Capsule = Remote Outposts**, **Hosts = Airplanes**.

---

## 🔹 Common Interview Questions on Red Hat Satellite

1. **What is Red Hat Satellite and why is it used?**
   → To manage RHEL systems at scale (provisioning, patching, configuration, subscriptions).

2. **Difference between Satellite 5 and Satellite 6?**

   * Satellite 5 → based on Spacewalk (older).
   * Satellite 6 → redesigned, based on **Foreman, Katello, Candlepin, and Pulp**.

3. **What are Capsules in Satellite?**
   → Capsules act as proxies for content, provisioning, and configuration in remote locations.

4. **How does patch management work in Satellite?**
   → Sync repos → Promote through lifecycle environments → Apply to hosts.

5. **What lifecycle environments are in Satellite?**
   → Typically: **Dev → Test → Prod** (you control the flow of patches).

6. **How does Satellite integrate with Ansible?**
   → Satellite 6 can execute Ansible playbooks and roles for host configuration.

7. **How do you register a host to Satellite?**

   * Use `subscription-manager register` with org + activation key.
   * Host reports to Satellite for updates/configuration.

8. **How does Satellite help with compliance/security?**
   → Provides OpenSCAP (Security Content Automation Protocol) scanning and compliance reporting.

9. **What are Activation Keys in Satellite?**
   → Pre-defined templates to register new systems with correct subscriptions, repos, and configs automatically.

10. **What databases and services back Satellite?**
    → PostgreSQL (database), Pulp (content), Katello (content & subscription), Candlepin (subscription service), Foreman (provisioning).

---

## 🔹 Hands-On Skills to Practice for Satellite Interview

If you have a test RHEL lab, try these:

* Install and configure a Red Hat Satellite server (trial subscription available).
* Sync repositories from Red Hat CDN.
* Create **Lifecycle Environments** (Dev → Test → Prod).
* Register a client system with `subscription-manager`.
* Apply patches via Satellite.
* Create an **Activation Key** and test provisioning.
* Configure a **Capsule Server** (if multiple sites).
* Run an **Ansible role** from Satellite to enforce config.
* Generate a compliance/security report.

---

✅ **Summary for interview prep:**
Red Hat Satellite is **not just a patch server** → it’s a **full lifecycle management solution for RHEL systems**. 
Learn **patching, provisioning, Ansible integration, subscription management, and architecture differences between Satellite 5 vs 6**.

---
Perfect ✅ — let’s build a **Red Hat Satellite 6 Interview Q\&A Cheat Sheet** so you can revise quickly.

---

# 🔹 Red Hat Satellite 6 Interview Q\&A

## 1. Basics

**Q1. What is Red Hat Satellite?**
👉 A system management platform to deploy, configure, patch, and monitor RHEL systems at scale.

**Q2. Difference between Satellite 5 and Satellite 6?**

* **Satellite 5** → Based on Spacewalk, older, Oracle DB.
* **Satellite 6** → Based on **Foreman, Katello, Pulp, Candlepin**, PostgreSQL, supports Ansible integration, lifecycle environments.

---

## 2. Architecture & Components

**Q3. What are the main components of Satellite 6?**

* **Foreman** → Provisioning and orchestration.
* **Katello** → Content and subscription management.
* **Pulp** → Repository and content storage.
* **Candlepin** → Subscription service.
* **Capsule Server** → Acts as proxy for remote sites.
* **PostgreSQL** → Database.

**Q4. What is a Capsule Server?**
👉 Acts as a **content, provisioning, and configuration proxy**. Used for remote datacenters or large environments.

**Q5. Explain Lifecycle Environments in Satellite.**
👉 Stages for patch/content promotion: **Development → Testing → Production**. Helps control patch rollout.

---

## 3. Content & Subscription Management

**Q6. What is Content View in Satellite?**
👉 A curated collection of repositories, packages, and updates. Can be versioned and promoted across lifecycle environments.

**Q7. What are Activation Keys?**
👉 Predefined keys used to register systems automatically with correct subscriptions, repos, and configs.

**Q8. How are subscriptions managed in Satellite?**
👉 Using **Candlepin** + Activation Keys → Satellite centrally manages Red Hat subscriptions and allocates them to hosts.

---

## 4. Provisioning

**Q9. How does Satellite provision systems?**
👉 Supports **Kickstart (PXE boot)**, VMware templates, OpenStack, AWS provisioning, bare-metal deployment.

**Q10. What’s the difference between Host and Host Group?**

* **Host** = Single managed system.
* **Host Group** = Template for provisioning (OS, subscriptions, parameters).

**Q11. What are Compute Resources in Satellite?**
👉 Virtualization and cloud platforms integrated with Satellite (e.g., VMware, OpenStack, AWS).

---

## 5. Configuration & Automation

**Q12. How does Satellite integrate with Ansible?**
👉 Satellite 6 can:

* Run Ansible roles and playbooks.
* Apply configuration policies to registered hosts.

**Q13. What are Puppet vs Ansible in Satellite?**
👉 Puppet was used in earlier Satellite versions. Ansible is now preferred for configuration automation.

---

## 6. Patch & Security

**Q14. How does patch management work in Satellite?**
👉 Repos are synced → added to Content View → promoted through Lifecycle Environments → applied to hosts.

**Q15. How to apply security updates only?**
👉 Use **errata filtering** (Security, Bugfix, Enhancement).

**Q16. How does Satellite help in compliance?**
👉 Via **OpenSCAP** integration (SCAP policies, security scanning, compliance reports).

---

## 7. Hands-On Operations

**Q17. How to register a system to Satellite?**

```bash
subscription-manager register --org="ORG_NAME" --activationkey="KEY_NAME"
```

**Q18. How to check if a host is properly registered?**
👉 In Satellite UI → Hosts → All Hosts, or CLI:

```bash
subscription-manager status
```

**Q19. How to sync repositories manually?**
👉 From Satellite UI (Content → Sync) or CLI:

```bash
hammer repository synchronize --id <repo_id>
```

**Q20. How to promote a Content View version to Production?**
👉 UI: Content → Content Views → Promote → Select Lifecycle Env.
👉 CLI:

```bash
hammer content-view version promote --id <cv_version_id> --to-lifecycle-environment Production
```

---

## 8. Troubleshooting

**Q21. A host is not receiving updates — what do you check?**

* Is it registered with Satellite? (`subscription-manager status`)
* Is repo enabled? (`subscription-manager repos --list-enabled`)
* Are lifecycle/content views properly published?

**Q22. Capsule server is not syncing — steps to debug?**

* Check connectivity to Satellite.
* Verify pulp services (`systemctl status`).
* Run `hammer capsule content synchronize`.

**Q23. What logs are important in Satellite troubleshooting?**

* `/var/log/foreman/` (Foreman logs).
* `/var/log/candlepin/` (subscription issues).
* `/var/log/pulp/` (repo sync issues).
* `/var/log/foreman-proxy/` (capsule issues).

---

## 9. Advanced

**Q24. How do Satellite and Insights work together?**
👉 Satellite integrates with **Red Hat Insights** to provide predictive analytics and remediation playbooks.

**Q25. What database does Satellite use?**
👉 PostgreSQL.

**Q26. Can Satellite manage non-RHEL systems?**
👉 Yes (limited), e.g., Fedora, CentOS, Ubuntu — but primary focus is RHEL.

**Q27. What is Katello-agent?**
👉 Used for remote host actions (package installs, updates). Replaced with **Remote Execution + Ansible** in newer versions.

**Q28. What is Foreman Discovery plugin?**
👉 Auto-discovers bare-metal hosts and registers them to Satellite for provisioning.

---

# 🔹 Tips for Interview

* Emphasize **real-world workflows**: patching, provisioning, lifecycle management.
* Know **differences between Satellite 5 & 6**.
* Be ready for **troubleshooting scenarios**.
* Practice `hammer` CLI commands (very common in interviews).

---
Got it 👍
If you want to study **Red Hat Satellite Server**, it helps to break it into **core areas**:
👉 Content management (repos, packages, errata)
👉 Hosts lifecycle (registering, provisioning, patching)
👉 Users & roles (RBAC, organizations, locations)
👉 Capsule/Smart Proxies (distributed content & services)

Here’s a **cheat sheet of useful commands** you’ll encounter when working with Satellite (uses the `hammer` CLI and some system tools):

---

## 🔹 General Satellite Server Commands

```bash
# Check Satellite version
rpm -q satellite

# Check status of Satellite services
satellite-maintain service status

# Start/Stop/Restart all services
satellite-maintain service restart
satellite-maintain service stop
satellite-maintain service start

# Run health check
satellite-maintain health check
```

---

## 🔹 Hammer CLI (main Satellite management tool)

### Authentication & Info

```bash
# Test hammer connection
hammer ping

# Check current user info
hammer user info --login admin
```

### Organizations & Locations

```bash
# List organizations
hammer organization list

# List locations
hammer location list
```

### Content Views & Repositories

```bash
# List available repositories
hammer repository list --organization "MyOrg"

# Sync a repository
hammer repository synchronize --name "BaseOS" --organization "MyOrg" --product "RHEL"

# List content views
hammer content-view list --organization "MyOrg"

# Publish a content view
hammer content-view publish --name "RHEL-CV" --organization "MyOrg"

# Promote content view to lifecycle environment
hammer content-view version promote --content-view "RHEL-CV" --version "1.0" \
  --to-lifecycle-environment "Production" --organization "MyOrg"
```

### Lifecycle Environments

```bash
# List lifecycle environments
hammer lifecycle-environment list --organization "MyOrg"
```

### Hosts

```bash
# List registered hosts
hammer host list --organization "MyOrg"

# Check host details
hammer host info --name host1.example.com

# Assign a content view to host
hammer host update --name host1.example.com \
  --content-view "RHEL-CV" \
  --lifecycle-environment "Production"
```

### Activation Keys

```bash
# List activation keys
hammer activation-key list --organization "MyOrg"

# Create activation key
hammer activation-key create --name "RHEL-Key" --organization "MyOrg" \
  --lifecycle-environment "Development" --content-view "RHEL-CV"
```

---

## 🔹 Katello & Capsule Related

```bash
# Check subscription status
subscription-manager status

# List consumed subscriptions
subscription-manager list --consumed

# Capsule sync
hammer capsule content synchronize --name "capsule01.example.com"
```

---

## 🔹 Backup & Restore

```bash
# Create backup
satellite-maintain backup online /backup/location

# Restore backup
satellite-maintain restore /backup/location
```

---

✅ **Tip for studying**:

* Practice in a lab environment (at least 2 VMs: one Satellite + one client/host).
* Focus on **content lifecycle**: Import repos → Create content views → Publish & Promote → Register hosts → Apply updates.
* The `hammer` command is the most useful for day-to-day management.

---
Perfect 👍 You’ll spend most of your time with **hammer CLI** and **satellite-maintain** on Red Hat Satellite. Let’s extend the list into a **comprehensive CLI reference** with more commands grouped by task area:

---

# 🔹 Satellite CLI Commands (Extended Cheat Sheet)

---

## 📌 System / Maintenance

```bash
# Check Satellite version and build
satellite-installer --version

# Check enabled repos on Satellite
yum repolist enabled

# Verify system status
satellite-maintain health check

# Check logs
journalctl -u tomcat
journalctl -u foreman
journalctl -u dynflow-sidekiq@*

# Cleanup orphaned tasks
foreman-rake foreman_tasks:cleanup TASK_SEARCH='state = stopped' DAYS_TO_KEEP=7
```

---

## 📌 Hammer CLI – Organizations, Locations, Users

```bash
# Create organization
hammer organization create --name "Finance" --label finance

# Create location
hammer location create --name "Mumbai-DC"

# List users
hammer user list

# Create user
hammer user create --login satadmin --mail admin@example.com --password redhat \
  --organization-ids 1 --location-ids 1
```

---

## 📌 Content & Repositories

```bash
# List products
hammer product list --organization "MyOrg"

# Create product
hammer product create --name "RHEL" --organization "MyOrg"

# Create repository
hammer repository create --name "RHEL8-BaseOS" \
  --content-type yum --product "RHEL" \
  --url "http://cdn.redhat.com/content/dist/rhel8/8/x86_64/baseos/os" \
  --organization "MyOrg"

# Sync all repos of an org
hammer repository synchronize --organization "MyOrg" --async --id 1
```

---

## 📌 Content Views & Composite Content Views

```bash
# Publish new version of content view
hammer content-view publish --name "RHEL-CV" --organization "MyOrg" --description "Updated CV"

# Promote version to next environment
hammer content-view version promote \
  --content-view "RHEL-CV" --version 2.0 \
  --to-lifecycle-environment "QA" --organization "MyOrg"

# Create composite content view
hammer content-view create --composite --name "FullStack" --organization "MyOrg"

# Add content view to composite
hammer content-view component add \
  --composite-content-view "FullStack" \
  --organization "MyOrg" --content-view "RHEL-CV" --latest
```

---

## 📌 Hosts & Host Groups

```bash
# Register a new host (client-side)
subscription-manager register --org="MyOrg" --activationkey="RHEL-Key"

# List all hosts
hammer host list --organization "MyOrg"

# Update host parameters
hammer host update --name "host1.example.com" --comment "Production server"

# List host groups
hammer host-group list --organization "MyOrg"

# Create host group
hammer host-group create --name "RHEL8-Prod" --organization "MyOrg" \
  --lifecycle-environment "Production" --content-view "RHEL-CV"
```

---

## 📌 Subscriptions & Activation Keys

```bash
# List subscriptions in org
hammer subscription list --organization "MyOrg"

# Add subscription to activation key
hammer activation-key add-subscription \
  --name "RHEL-Key" --organization "MyOrg" --subscription-id 3

# List subscriptions consumed by host
hammer host subscription list --host host1.example.com
```

---

## 📌 Errata & Patching

```bash
# List errata available
hammer erratum list --organization "MyOrg"

# Apply errata to host
hammer host errata apply --host "host1.example.com" --errata-id "RHBA-2025:1234"

# List errata applicable to a host
hammer host errata list --host "host1.example.com"
```

---

## 📌 Capsule / Smart Proxy

```bash
# List capsules
hammer capsule list

# Sync capsule content
hammer capsule content synchronize --id 2

# Check capsule features
hammer capsule info --id 2
```

---

## 📌 Tasks & Jobs

```bash
# List tasks
hammer task list

# Show task details
hammer task info --id <TASK_ID>

# Rerun failed tasks
foreman-rake foreman_tasks:retry_failed
```

---

## 📌 Backup & Restore

```bash
# Online backup
satellite-maintain backup online /backup/location --assumeyes

# Offline backup
satellite-maintain backup offline /backup/location

# Restore backup
satellite-maintain restore /backup/location
```

---

⚡ With these commands, you can:

* **Build full repo lifecycle** → Product → Repository → Sync → Content View → Promote
* **Manage hosts easily** → Register → Assign CV → Patch → Errata apply
* **Operate Satellite itself** → Check health, logs, tasks, backups

---

👉 Do you want me to also prepare a **hands-on lab guide** (step-by-step exercises) where you’ll use these commands in sequence (from installing Satellite → creating repos → registering a host → patching it)? That will help you *practice* instead of just memorizing.
