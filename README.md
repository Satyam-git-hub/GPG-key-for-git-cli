# 🔐 GitHub Verified Commits with GPG

This guide walks through generating a **GPG key**, adding it to **GitHub**, and using it to **sign commits** for verified signatures. It also includes solutions for common issues during the process.

---

## 📌 Step 1: Generate a New GPG Key
Run the following command to generate a **GPG key**:
```sh
gpg --full-generate-key
```
### **🔹 Select the Options:**
1. **Algorithm:** Choose `RSA and RSA` (option 1).
2. **Key Size:** Select `4096` bits.
3. **Expiration:** Choose an expiration date or `0` for no expiry.
4. **User Information:**
   - **Real Name:** (Your name as it appears on GitHub)
   - **Email Address:** (Use the same email as your GitHub account)
   - **Comment:** (Optional)
5. **Passphrase:** Set a secure passphrase.

After generation, list your secret keys:
```sh
gpg --list-secret-keys --keyid-format=long
```
Example output:
```
/home/user/.gnupg/pubring.kbx
---------------------------------
sec   rsa4096/41766A3CE7CBD6AD 2025-03-06 [SC] [expires: 2025-06-04]
      3B5E4255A6963C321016A50F41766A3CE7CBD6AD
uid                 [ultimate] Your Name <your.email@example.com>
ssb   rsa4096/5C876ACACA6B20B1 2025-03-06 [E] [expires: 2025-06-04]
```
### **✅ Copy Your GPG Key ID**
Find the `sec` line and **copy** the long key ID (`41766A3CE7CBD6AD` in this case).

---

## 📌 Step 2: Add Your GPG Key to GitHub
### **🔹 Export the Public Key**
Run:
```sh
gpg --armor --export 41766A3CE7CBD6AD
```
Copy the output and add it to **GitHub**:

1. Go to **GitHub → Settings → SSH and GPG keys**.
2. Click **New GPG Key**.
3. **Paste the exported key** and **Save**.

---

## 📌 Step 3: Configure Git to Sign Commits
Run the following:
```sh
git config --global user.signingkey 41766A3CE7CBD6AD
git config --global commit.gpgsign true
```
✅ This enables GPG signing for all commits.

---

## 📌 Step 4: Verify and Sign a Commit
Test signing manually:
```sh
echo "test" | gpg --clearsign
```
If successful, try a **signed Git commit**:
```sh
git commit -S -m "My first signed commit"
git push origin main
```
🚀 Now, your commits should appear as **Verified** on GitHub.

---

# ❌ Troubleshooting GPG Issues
### 🔴 **1. `gpg failed to sign the data`**
Run:
```sh
gpg-agent --daemon
```
Then retry the commit.

---

### 🔴 **2. `gpg: signing failed: Inappropriate ioctl for device`**
If using **VS Code, SSH, or WSL**, set:
```sh
export GPG_TTY=$(tty)
echo "export GPG_TTY=$(tty)" >> ~/.bashrc
source ~/.bashrc
```

---

### 🔴 **3. `No secret key` or `gpg: signing failed`**
Check if your key is available:
```sh
gpg --list-secret-keys --keyid-format=long
```
If missing, re-import:
```sh
gpg --import ~/.gnupg/secring.gpg
```

---

### 🔴 **4. `gpg: decryption failed: No secret key`**
Try restarting the GPG agent:
```sh
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

---

### 🔴 **5. `git commit -S` fails in GUI apps**
Allow **loopback pinentry**:
```sh
echo "use-agent" >> ~/.gnupg/gpg.conf
echo "pinentry-mode loopback" >> ~/.gnupg/gpg.conf
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

---

# ✅ Verifying Signed Commits
Check commit signature:
```sh
git log --show-signature
```
Output should confirm **GPG signed commit**.

---

# 🔥 Alternative: GitHub Web UI for Verified Commits
If you want an **easier way**, use GitHub’s **web editor**:
1. Open a repository.
2. Edit a file and commit.
3. GitHub automatically **signs** your commits as **Verified**.

🚀 Now your commits will always be secure and verified! 🎯



