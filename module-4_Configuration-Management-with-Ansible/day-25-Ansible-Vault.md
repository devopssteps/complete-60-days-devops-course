### Day-25 : Ansible Vault (Secret Management)

"Ansible Vault (Secret Management)" is a vital DevOps skill for managing sensitive data like passwords, API keys, and credentials securely in Ansible projects.

Here's everything you need:
 - ✅ Clear explanation
 - 🛠️ Hands-on demo

### ✅ What is Ansible Vault?
Ansible Vault allows you to encrypt sensitive variables and files within your Ansible projects so that secrets (e.g., passwords, tokens) are not stored in plain text.

You can encrypt:
 - Entire files (e.g., vault.yml)
 - Specific variables within files
 - Roles or vars files 

## 🛠️ Hands-On: Ansible Vault in Action
### ✅ Step 1: Create a Secure Variable File
```sh
ansible-vault create secrets.yml
```
This will open your default editor. Add something like:
```sh
db_password: "SuperSecret123"
```
Save and exit. Now the file is encrypted.

### ✅ Step 2: Use Encrypted Vars in a Playbook
playbook.yml
```sh
- name: Use Ansible Vault
  hosts: localhost
  vars_files:
    - secrets.yml
  tasks:
    - name: Show Encrypted Password
      debug:
        msg: "The DB password is {{ db_password }}"
```

### ✅ Step 3: Run the Playbook
```sh
ansible-playbook playbook.yml --ask-vault-pass
```

Enter your vault password when prompted. You’ll see:
```sh
TASK [Show Encrypted Password]
ok: [localhost] => {
    "msg": "The DB password is SuperSecret123"
}
```
### ✅ Step 4: Vault Commands You Should Know
| Command                                                           | Description               |
| ----------------------------------------------------------------- | ------------------------- |
| `ansible-vault create <your-playbook-file-name ex: secret.yaml>`  | Create an encrypted file  |
| `ansible-vault edit <your-playbook-file-name ex: secret.yaml>`    | Edit the encrypted file   |
| `ansible-vault view <your-playbook-file-name ex: secret.yaml>`    | View encrypted content    |
| `ansible-vault encrypt <your-playbook-file-name ex: secret.yaml>` | Encrypt an existing file  |
| `ansible-vault decrypt <your-playbook-file-name ex: secret.yaml>` | Decrypt a file            |
| `ansible-vault rekey <your-playbook-file-name ex: secret.yaml>`   | Change the vault password |

### 🧠 Pro Tips:
 - Avoid putting secrets directly into playbooks.
 - Use group_vars/ or host_vars/ with vault-encrypted files.
 - For automation, use --vault-password-file instead of --ask-vault-pass.

✅ This way, you keep passwords safe in Git repos without exposing them in plain text.

