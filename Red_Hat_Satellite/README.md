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
