# Testing - Vault
## What is Vault?
Vault is a tool that helps you store and manage sensitive information like:
- Passwords
- API keys
- Certificates
- Database credentials

![DigitalVaultNEw](https://github.com/user-attachments/assets/af27d076-4873-4594-8d50-e050d168749e)



# Installation Steps for Vault on Ubuntu Server:

## To install HashiCorp Vault on Ubuntu, follow these steps:

### ✅ Step 1: Update System
```
 sudo apt update && sudo apt upgrade -y
```
### ✅ Step 2: Install Required Dependencies
```
 sudo apt install -y wget gnupg software-properties-common
```
### ✅ Step 3: Add HashiCorp GPG Key
```
 wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```
### ✅ Step 4: Add the HashiCorp APT Repository
```
 echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```
### ✅ Step 5: Install Vault
```
 sudo apt update
 sudo apt install vault -y
```
 ### ✅ Step 6: Verify Installation
 ```
  vault --version
```
You should see something like:
```
 Vault v1.x.x
```
### ✅ Access Vault via Web UI
To access HashiCorp Vault using your public IP, follow these steps carefully. This setup assumes you're running Vault on an Ubuntu server that has a public IP 
#### 1. Edit the Vault Configuration File
```
 sudo nano /etc/vault.d/vault.hcl
```
Make sure the listener block is configured like this:
```
 ui = true

listener "tcp" {
  address     = "0.0.0.0:8200"  # Listen on all interfaces, including public IP
  tls_disable = 1               # Only for testing! Use TLS in production
}

storage "file" {
  path = "/var/lib/vault/data"
}
```
🔒 Note: Disabling TLS is insecure for production. Use TLS certificates when exposing Vault over the internet.
### 2. Restart the Vault Service
```
 sudo systemctl daemon-reexec
sudo systemctl restart vault
```
### 3. ✅ Solution: Fix Permissions for Vault Storage Directory
Step 1: Create the Vault Data Directory (if it doesn't exist)
```
 sudo mkdir -p /var/lib/vault
```
Step 2: Set the Correct Ownership
```
 sudo chown -R vault:vault /var/lib/vault
```
Step 3: Restart Vault
```
 sudo systemctl restart vault
```
### 4. Access from Browser via ip address
```
 http://<your-public-ip>:8200/ui
```
