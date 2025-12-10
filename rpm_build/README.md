
# 1. Steps for jenkins user to sign user with gpg key.

### Fix Steps

1. **Switch to Jenkins user** (the one running Jenkins, usually `jenkins`):

   ```bash
   sudo su - jenkins
   ```

2. **Check for existing secret keys**:

   ```bash
   gpg --list-secret-keys
   ```

   If nothing is shown → no key is available.

3. **Generate a new GPG key (if needed)**:

   ```bash
   gpg --full-generate-key
   ```

   * Key type: RSA and RSA
   * Key size: 4096 (recommended)
   * Expiry: choose as per your policy
   * Add name, email, etc.

   After generation, check:

   ```bash
   gpg --list-secret-keys --keyid-format=long
   ```

   You should see something like:

   ```
   sec   rsa4096/ABCDEF1234567890 2025-08-18 [SC]
   ```

   Here `ABCDEF1234567890` is your **GPG Key ID**.

4. **Export the public key** (to distribute if others need verification):

   ```bash
   gpg --armor --export ABCDEF1234567890 > RPM-GPG-KEY-myorg
   ```

5. **Configure Jenkins**:

   * Go to **Manage Jenkins → Configure System → RPM Sign Plugin**.
   * Enter:

     * **GPG Executable** → usually `/usr/bin/gpg`
     * **Key Name/ID** → your key ID (`ABCDEF1234567890`)
     * **Passphrase** → (set in Jenkins credentials, referenced by the plugin).

6. **Test signing manually** (as Jenkins user):

   ```bash
   rpmsign --addsign mypackage.rpm
   ```

   If this works → the plugin should also work.

---

### Common Pitfalls

* Jenkins runs in a limited environment, so your GPG key may be in **root’s keyring**, not Jenkins’s. Keys must be created or imported under the **jenkins** user.
* If your key requires a passphrase, you need to configure Jenkins credentials and link them in the plugin.
* If you used `~/.rpmmacros` with `%_gpg_name`, make sure it matches the key ID.

---

👉 Do you already have a GPG key created outside Jenkins (e.g., under root), or should I walk you through moving/importing that key into the Jenkins user’s keyring?
---
Perfect 👍 — you already have a valid GPG secret key under the **jenkins** user:

* **Key ID (short)**: `B2ED5BE4810473CA`
* **Fingerprint (long)**: `97D7B20753A3E4829879D7C9B2ED5BE48834D3CA`
* **User ID**: `Suresh Borse <s.borse@example.biz>`

That means the failure is not because the key doesn’t exist, but because Jenkins (the **RPM Sign Plugin**) doesn’t know how to use it.

---

# 2. What you need to do:

#### 1. Configure `.rpmmacros`

Make sure the Jenkins user has a proper `~/.rpmmacros` file:

```bash
cat > /var/lib/jenkins/.rpmmacros <<EOF
%_signature gpg
%_gpg_name B2ED5BE48834D3CA
%__gpg /usr/bin/gpg
EOF
```

> `%_gpg_name` can be either the **short key ID** (`B2ED5BE48834D3CA`) or the full fingerprint.

#### 2. Test signing manually

Still as **jenkins** user, run:

```bash
rpmsign --addsign /path/to/your.rpm
```

* If it asks for a **passphrase** and succeeds → you’re good.
* If it fails with "No key" → check the `%_gpg_name` in `.rpmmacros`.

#### 3. Store Passphrase in Jenkins

If your key has a passphrase (most likely yes):

* Go to **Manage Jenkins → Credentials**
* Add a **“Secret text”** credential with your GPG passphrase.
* Go to **Manage Jenkins → Configure System → RPM Sign Plugin**:

  * **GPG Executable** → `/usr/bin/gpg`
  * **Key Name** → `B2ED5BE48834D3CA`
  * **Passphrase** → select the credential you created.

#### 4. Retry Jenkins job

Run the job again. Now it should find the key and sign.

---

⚠️ Common issues:

* If Jenkins still says *“No GPG key is available”*, it usually means:

  * The plugin is looking in the wrong homedir.
    Try setting in **Jenkins → Manage Jenkins → Configure System → Global properties → Environment variables**:

    ```
    GNUPGHOME=/var/lib/jenkins/.gnupg
    ```

  * Or the `%_gpg_name` doesn’t exactly match your key ID.

---

👉 Do you want me to give you a **working minimal Jenkins + RPM Sign plugin config snippet** (with the key ID and passphrase reference), so you can copy-paste into your Jenkins config?


---
# 3. Below is a **step-by-step, clean, production-ready guide** to help you **build your own custom `httpd` RPM** on **AlmaLinux 8/9**.
After building, you can store this RPM in your internal repository and use it to update all your AlmaLinux servers.

---

### ✅ **1. Install Required Build Tools**

```bash
sudo dnf groupinstall "Development Tools" -y
sudo dnf install rpm-build rpmdevtools yum-utils dnf-plugins-core -y
sudo dnf install pinentry pinentry-curses -y
```

Create the standard RPM build directories:

```bash
rpmdev-setuptree
```

This creates:

```
~/rpmbuild/
  ├── SPECS
  ├── SOURCES
  ├── RPMS
  ├── SRPMS
  ├── BUILD
  └── BUILDROOT
```

---

### ✅ **2. Download Apache HTTPD Source Code (SRC.RPM)**

The easiest way to build `httpd` for Alma is to download the official AlmaLinux SRPM and modify/rebuild it.

### For AlmaLinux 9:

```bash
sudo dnf download --source httpd
```

You will get something like:

```
httpd-2.4.57-1.el9.src.rpm
```

Install the SRPM:

```bash
rpm -ivh httpd-*.src.rpm
```

This writes:

* `~/rpmbuild/SPECS/httpd.spec`
* Tarballs/patches into `~/rpmbuild/SOURCES/`

---

### ✅ **3. Install Build Dependencies**

```bash
sudo dnf builddep ~/rpmbuild/SPECS/httpd.spec -y
```

This automatically installs all required packages (APR, PCRE, OpenSSL-devel, etc.)

---

### ✅ **4. (Optional) Modify SPEC File**

Edit the spec file:

```bash
vim ~/rpmbuild/SPECS/httpd.spec
```

You can optionally update:

* version
* patches
* custom compilation flags
* custom modules
* change ServerTokens, ServerSignature defaults
* disable unwanted modules (mod_ldap, mod_proxy, etc.)

For example, to increase default worker processes, add custom patch, etc.

---

### ✅ **5. Build the RPM**

Use the standard rpmbuild command:

```bash
rpmbuild -ba ~/rpmbuild/SPECS/httpd.spec
```

This will generate:

```
~/rpmbuild/RPMS/x86_64/httpd-*.rpm
~/rpmbuild/SRPMS/httpd-*.src.rpm
```

---

### ✅ **6. Install Your Custom HTTPD**

```bash
sudo rpm -Uvh ~/rpmbuild/RPMS/x86_64/httpd-*.rpm
```

---

### ✅ **7. (Optional) Create a Local YUM Repository**

If you want all servers to update from your custom httpd RPM:

### Create repo directory:

```bash
sudo mkdir -p /var/www/html/repo/httpd
sudo cp ~/rpmbuild/RPMS/x86_64/*.rpm /var/www/html/repo/httpd/
```

### Generate repo metadata:

```bash
sudo dnf install createrepo -y
sudo createrepo /var/www/html/repo/httpd
```

### Create repo file:

```bash
sudo tee /etc/yum.repos.d/custom-httpd.repo <<EOF
[custom-httpd]
name=Custom HTTPD Repo
baseurl=http://your-server/repo/httpd
enabled=1
gpgcheck=0
EOF
```

---

### ✅ **8. Test the Repo**

On any AlmaLinux server:

```bash
sudo dnf clean all
sudo dnf repolist
sudo dnf update httpd
```

Your custom `httpd` should now be installed.

---

### 🔐 **(Optional) Sign Your RPM with GPG**

If you want secure updates:

### Generate a GPG Key:

```bash
gpg --full-generate-key
```

Find key name:

```bash
gpg --list-keys --keyid-format=long
```

Add to `~/.rpmmacros`:

```
%_gpg_name Your-Key-ID-Here
```

Sign RPM automatically:

```
echo "%_signature gpg" >> ~/.rpmmacros
echo "%_gpg_path ~/.gnupg" >> ~/.rpmmacros
echo "%_gpgbin /usr/bin/gpg" >> ~/.rpmmacros
echo "%_gpg_digest_algo sha256" >> ~/.rpmmacros
```

Sign the built RPM:

```bash
rpm --addsign ~/rpmbuild/RPMS/x86_64/httpd-*.rpm
```

---

# 🎉 Done!

You now have a **complete custom HTTPD RPM build pipeline**, suitable for enterprise deployment across all your AlmaLinux servers.

---

# ⭐ If you want:

✔ a custom spec file
✔ a modified httpd including custom modules
✔ RPM signing fixes (your previous `%_gpg_name` error)
✔ Jenkins pipeline to build the RPM
✔ Push RPM to Nexus / Artifactory
✔ Host a private YUM repo on Apache

Just tell me — I can prepare the complete working setup for you.
---

# 4. what is key id in the following output

```bash
pub   rsa3072/4823525D0582CD71 2025-12-10 [SC] [expires: 2026-12-10]
      F768E878CBACF31402308D024823525D0582CD71
uid                 [ultimate] jay@localhost (local key) <jay@localhost>
sub   rsa3072/F19BC1ACC97FA1E8 2025-12-10 [E] [expires: 2026-12-10]
```
---
Your **Key ID** and **Fingerprint** are both shown in your output — but they are different things.

Let me break them down clearly:

---

### ✅ **1. Short Key ID**

From this part:

```
rsa3072/4823525D0582CD71
```

The **short Key ID** is:

```
0582CD71
```

---

### ✅ **2. Long Key ID**

The characters after the slash are the **long key ID**:

```
4823525D0582CD71
```

---

### ✅ **3. Full Fingerprint**

Below this line:

```
F768E878CBACF31402308D024823525D0582CD71
```

This is the **full 40-character fingerprint**.

---

### ⭐ Which one to use in RPM signing?

For **~/.rpmmacros**, you MUST use the **full fingerprint**:

```
%_gpg_name F768E878CBACF31402308D024823525D0582CD71
```

OR you can use the **long Key ID**:

```
%_gpg_name 4823525D0582CD71
```

Both work, but **full fingerprint is recommended**.

---

### 🔍 Summary

| Type                       | Value                                      |
| -------------------------- | ------------------------------------------ |
| **Short Key ID**           | `0582CD71`                                 |
| **Long Key ID**            | `4823525D0582CD71`                         |
| **Full FPR (recommended)** | `F768E878CBACF31402308D024823525D0582CD71` |

---

If you want, I can now help you configure your **~/.rpmmacros** correctly for RPM signing.
---
You are **almost done** — but your `~/.rpmmacros` file is **missing one important line**:

✔ **%_gpg_name** → This tells `rpmbuild` which GPG key to use for signing.

Without this, you will get errors like:

```
gpg: signing failed: No secret key
```

or

```
error: Macro % has illegal name (%define)
```

---

### ✅ **Correct ~/.rpmmacros (working example)**

Use your **full fingerprint** (recommended):

```
%_signature gpg
%_gpg_path ~/.gnupg
%_gpgbin /usr/bin/gpg
%_gpg_digest_algo sha256
%_gpg_name F768E878CBACF31402308D024823525D0582CD71
```

Or use **long Key ID** (also works):

```
%_gpg_name 4823525D0582CD71
```

---

### 🚀 After adding this, test RPM signing:

```bash
rpm --addsign /path/to/your.rpm
```

Then verify:

```bash
rpm --checksig /path/to/your.rpm
```

Expected output:

```
your.rpm: rsa sha256 (md5) pgp md5 OK
```

---

# 5. 👍 Your file is almost correct — just add the `%_gpg_name` line!

If you want, I can also help you automate RPM signing in Jenkins or while building `httpd` RPM.
```bash

Import the exported public key into rpm database as follows.
[root@localhost ~]# rpm --import RPM-GPG-KEY-pmanager

[root@localhost ~]# rpm -q gpg-pubkey --qf '%{name}-%{version}-%{release} --> %{summary}\n'
        gpg-pubkey-fd431d51-4ae0493b --> gpg(Red Hat, Inc. (release key 2) <security@redhat.com>)
        gpg-pubkey-2fa658e0-45700c69 --> gpg(Red Hat, Inc. (auxiliary key) <security@redhat.com>)
        gpg-pubkey-b74246ce-58d281c9 --> gpg(Package Manager (RPM Signing Key) <pmanager@example.com>)
```	
---
You're getting:

```
gpg: WARNING: nothing exported
```

and your file is empty because **the key name you used (‘Package Manager’) does NOT match any key in your keyring**.

You must export using the **exact UID, Key ID, or Fingerprint**.

Let me show you exactly how to fix it.

---

### ✅ Step 1 — View your actual key

Run:

```bash
gpg --list-keys --keyid-format=long
```

From your earlier message, your key looks like:

```
pub   rsa3072/4823525D0582CD71 2025-12-10 [SC] [expires: 2026-12-10]
      F768E878CBACF31402308D024823525D0582CD71
uid                 [ultimate] jay@localhost (local key) <jay@localhost>
sub   rsa3072/F19BC1ACC97FA1E8 2025-12-10 [E] [expires: 2026-12-10]
```

So your UID is:

```
jay@localhost (local key) <jay@localhost>
```

And your **fingerprint** is:

```
F768E878CBACF31402308D024823525D0582CD71
```

---

### ✅ Step 2 — Export using one of these correct methods

### 🔹 **Option 1: Export using UID name**

```bash
gpg --export -a "jay@localhost (local key)" > RPM-GPG-KEY-jay
```

### 🔹 **Option 2: Export using email**

```bash
gpg --export -a "jay@localhost" > RPM-GPG-KEY-jay
```

### 🔹 **Option 3: Export using long key ID**

```bash
gpg --export -a 4823525D0582CD71 > RPM-GPG-KEY-jay
```

### 🔹 **Option 4 (BEST): export using full fingerprint**

```bash
gpg --export -a F768E878CBACF31402308D024823525D0582CD71 > RPM-GPG-KEY-jay
```

---

### 🚀 Step 3 — Verify output file

```bash
file RPM-GPG-KEY-jay
```

Should show:

```
ASCII text
```

And check contents:

```bash
cat RPM-GPG-KEY-jay
```

You should see something starting with:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
```

---

### ❗ Why your command failed

You used:

```
gpg --export -a 'Package Manager'
```

But your key has no UID named “Package Manager”.

GPG only exports keys if the name/email matches **exactly**.

---

### If you want, I can also help you:

✔ Import this key into your internal YUM repo
✔ Sign RPMs automatically
✔ Configure rpm-signing inside Jenkins
✔ Build + sign custom HTTPD RPM end-to-end

Just tell me!
