# 🚀 Day 4 – GitHub Lab: Bare Repository, Permissions & Git Security

## 🎯 Objective

Copy a sample `index.html` file from the Jump Host into a cloned Git repository on the Storage Server, commit the file, and push it to a **local bare repository**, resolving real-world **Git ownership and permission issues**.
---
## 🧩 Lab Setup

| Component         | Path / User                     |
| ----------------- | ------------------------------- |
| Bare repository   | `/opt/media.git`                |
| Cloned repository | `/usr/src/kodekloudrepos/media` |
| Developer user    | `natasha`                       |
| Sample file       | `/tmp/index.html` (Jump Host)   |
| Branch            | `master`                        |

---

## 🛠️ Complete Command Flow (With Purpose)

---

## 🔹 STEP 1: Copy file from Jump Host to Storage Server

### On **Jump Host**

```bash
ls -l /tmp/index.html
scp /tmp/index.html natasha@ststor01:/tmp/
```

✔ `/tmp` is writable
✔ No permission issues

---

## 🔹 STEP 2: Move file into repository (root-only operation)

### On **Storage Server**

```bash
ssh natasha@ststor01
sudo su -
mv /tmp/index.html /usr/src/kodekloudrepos/media/
exit
```

✔ Repo directory is root-owned
✔ No permission changes made

---

## 🔹 STEP 3: Prepare Git identity (natasha)

```bash
cd /usr/src/kodekloudrepos/media
git config user.name "natasha"
git config user.email "natasha@stratos.xfusioncorp.com"
```

---

## 🚨 Issues Faced & Detailed Resolutions

---

## ❌ Issue 1: Permission denied while copying directly

**Error**

```
Permission denied
```

**Cause**

* `natasha` cannot write directly to repo directory

**Fix**
✔ Copy to `/tmp`
✔ Move as `root`

---

## ❌ Issue 2: Git “dubious ownership” (working repo)

**Error**

```
fatal: detected dubious ownership in repository
```

**Cause**

* Repo owned by a different user

**Fix**

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/media
```

---

## ❌ Issue 3: `.git/index.lock` permission denied

**Error**

```
Unable to create .git/index.lock
```

**Cause**

* Ownership mismatch in `.git` directory

**Fix**
✔ Trust repo using `safe.directory`
✔ Avoid chmod/chown (not allowed)

---

## 🔹 STEP 4: Add and commit file (natasha)

```bash
git add index.html
git commit -m "Add index.html file"
```

---

## ❌ Issue 4: Author identity unknown

**Error**

```
Author identity unknown
```

**Fix**

```bash
git config user.name "natasha"
git config user.email "natasha@stratos.xfusioncorp.com"
```

---

## ❌ Issue 5: Dubious ownership (remote bare repo)

**Error**

```
fatal: detected dubious ownership in repository at '/opt/media.git'
```

**Fix**

```bash
git config --global --add safe.directory /opt/media.git
```

---

## ❌ Issue 6: Remote unpack failed

**Error**

```
remote unpack failed: unable to create temporary object directory
```

**Cause**

* Bare repo owned by `root`
* Natasha cannot write objects

---

## 🔹 STEP 5: Push as root (server-side responsibility)

```bash
sudo su -
git config --global --add safe.directory /usr/src/kodekloudrepos/media
cd /usr/src/kodekloudrepos/media
git push origin master
```

✔ Push succeeds
✔ No ownership changes required

---

## ✅ Final Verification

```bash
git log --oneline -1
```

---

## 🧠 Key Learnings (DevOps-Level)

* Git enforces **ownership-based security**
* `safe.directory` is preferred over permission changes
* Bare repositories require **write access on server**
* Developers commit; infrastructure owners manage pushes
* Git errors often indicate **filesystem problems**, not Git misuse

---

## 📊 Architecture Summary

```
Jump Host
   |
   | scp
   ↓
Storage Server (/tmp)
   |
   | mv (root)
   ↓
Normal Repo (/usr/src/kodekloudrepos/media)
   |
   | git push
   ↓
Bare Repo (/opt/media.git)
```

---

## 🏁 Final Status

✔ File copied
✔ Committed successfully
✔ Pushed to bare repo
✔ All security checks resolved
---
<img width="231" height="218" alt="image" src="https://github.com/user-attachments/assets/74b33213-0c1d-4063-a3ab-7b102fbb8dae" />
<img width="625" height="370" alt="image" src="https://github.com/user-attachments/assets/5c31a1a5-3f03-4444-be08-9188abfa3239" />
<img width="236" height="182" alt="image" src="https://github.com/user-attachments/assets/3397ddee-d794-4032-933e-6e4a86a06ba8" />
<img width="360" height="280" alt="image" src="https://github.com/user-attachments/assets/cc9953dc-e685-4d13-b852-32a8c6546124" />


