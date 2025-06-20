# 🧪 Testing - Vault
## 🔐 What is Vault?
Vault is a tool that helps you store and manage sensitive information like:
- Passwords
- API keys
- Certificates
- Database credentials

![DigitalVaultNEw](https://github.com/user-attachments/assets/af27d076-4873-4594-8d50-e050d168749e)





## Before installing Vault, you need a server to run it. Here’s how to set up an Ubuntu server on AWS:
- Login to AWS Console and go to EC2 Dashboard.
- Click "Launch Instance".
- Choose AMI: Select Ubuntu Server 22.04 LTS (HVM), SSD Volume Type.
- Instance Type: Choose t2.micro (free tier) or a higher type if needed.
- Key Pair: Select or create a key pair to access your server.
- Network Settings:
Allow SSH (port 22)
Allow Custom TCP on port 8200 (needed for Vault UI)
- Launch Instance.
- After it’s running, connect to it using:

  ```
  ssh -i your-key.pem ubuntu@<your-public-ip>
```



# 🛠️ Installation Steps for Vault on Ubuntu Server

## To install HashiCorp Vault on Ubuntu, follow these steps:

### ✅ Step 1: Update System
Run a command to update the list of software packages and upgrade them to the latest version.

```
 sudo apt update && sudo apt upgrade -y
```
### ✅ Step 2: Install Required Dependencies
Install tools like wget, gnupg, and software-properties-common which are needed to securely add and manage software from external sources

```
 sudo apt install -y wget gnupg software-properties-common
```
### ✅ Step 3: Add HashiCorp GPG Key
Add the GPG security key from HashiCorp so Ubuntu can trust the Vault package during installation.

```
 wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```
### ✅ Step 4: Add the HashiCorp APT Repository
Add Vault’s official software source (repository) to your system so that you can install Vault directly from it.

```
 echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```
### ✅ Step 5: Install Vault
Update your system again to include the new repository and then install Vault using the package manager.

```
 sudo apt update
 sudo apt install vault -y
```
 ### ✅ Step 6: Verify Installation
 Run a command to check if Vault was installed correctly.

 ```
  vault --version
```
You should see something like:

```
 Vault v1.x.x
```
### ✅ Access Vault via Web UI
## To use Vault in a web browser:
To access HashiCorp Vault using your public IP, follow these steps carefully. This setup assumes you're running Vault on an Ubuntu server that has a public IP 
#### 1. Edit the Vault Configuration File
Open the Vault config file and make sure it allows the web UI and listens on all IP addresses (so it works via your public IP).

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
