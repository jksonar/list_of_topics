That error means your Jenkins **RPM Sign Plugin** is trying to sign your RPMs but can’t find a valid GPG key in the environment where Jenkins is running. Let’s break down why it happens and how to fix it:

---

### Why you’re seeing

```
java.lang.InterruptedException: No GPG key is available.
```

* The plugin relies on a **GPG private key** to sign RPMs.
* Jenkins either:

  * can’t see any imported key in the keyring (`gpg --list-secret-keys` returns empty for the Jenkins user),
  * or the plugin configuration doesn’t specify which key ID to use,
  * or Jenkins doesn’t have permission to access the GPG keyring.

---

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

* **Key ID (short)**: `B2ED5BE48834D3CA`
* **Fingerprint (long)**: `97D7B2D962A3E4829879D7C9B2ED5BE48834D3CA`
* **User ID**: `Suresh Borse <s.borse@direction.biz>`

That means the failure is not because the key doesn’t exist, but because Jenkins (the **RPM Sign Plugin**) doesn’t know how to use it.

---

### What you need to do:

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
what is key id 

pub   rsa3072/4823525D0582CD71 2025-12-10 [SC] [expires: 2026-12-10]
      F768E878CBACF31402308D024823525D0582CD71
uid                 [ultimate] jay@localhost (local key) <jay@localhost>
sub   rsa3072/F19BC1ACC97FA1E8 2025-12-10 [E] [expires: 2026-12-10]

---
Your **Key ID** and **Fingerprint** are both shown in your output — but they are different things.

Let me break them down clearly:

---

# ✅ **1. Short Key ID**

From this part:

```
rsa3072/4823525D0582CD71
```

The **short Key ID** is:

```
0582CD71
```

---

# ✅ **2. Long Key ID**

The characters after the slash are the **long key ID**:

```
4823525D0582CD71
```

---

# ✅ **3. Full Fingerprint**

Below this line:

```
F768E878CBACF31402308D024823525D0582CD71
```

This is the **full 40-character fingerprint**.

---

# ⭐ Which one to use in RPM signing?

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

# 🔍 Summary

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

# ✅ **Correct ~/.rpmmacros (working example)**

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

# 🚀 After adding this, test RPM signing:

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

# 👍 Your file is almost correct — just add the `%_gpg_name` line!

If you want, I can also help you automate RPM signing in Jenkins or while building `httpd` RPM.


Import the exported public key into rpm database as follows.
[root@localhost ~]# rpm --import RPM-GPG-KEY-pmanager

[root@localhost ~]# rpm -q gpg-pubkey --qf '%{name}-%{version}-%{release} --> %{summary}\n'
        gpg-pubkey-fd431d51-4ae0493b --> gpg(Red Hat, Inc. (release key 2) <security@redhat.com>)
        gpg-pubkey-2fa658e0-45700c69 --> gpg(Red Hat, Inc. (auxiliary key) <security@redhat.com>)
        gpg-pubkey-b74246ce-58d281c9 --> gpg(Package Manager (RPM Signing Key) <pmanager@example.com>)
		
---
You're getting:

```
gpg: WARNING: nothing exported
```

and your file is empty because **the key name you used (‘Package Manager’) does NOT match any key in your keyring**.

You must export using the **exact UID, Key ID, or Fingerprint**.

Let me show you exactly how to fix it.

---

# ✅ Step 1 — View your actual key

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

# ✅ Step 2 — Export using one of these correct methods

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

# 🚀 Step 3 — Verify output file

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

# ❗ Why your command failed

You used:

```
gpg --export -a 'Package Manager'
```

But your key has no UID named “Package Manager”.

GPG only exports keys if the name/email matches **exactly**.

---

# If you want, I can also help you:

✔ Import this key into your internal YUM repo
✔ Sign RPMs automatically
✔ Configure rpm-signing inside Jenkins
✔ Build + sign custom HTTPD RPM end-to-end

Just tell me!
