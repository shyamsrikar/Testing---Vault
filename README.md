# Testing - Vault
## What is Vault?
Vault is a tool that helps you store and manage sensitive information like:
- Passwords
- API keys
- Certificates
- Database credentials

![DigitalVaultNEw](https://github.com/user-attachments/assets/af27d076-4873-4594-8d50-e050d168749e)

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
