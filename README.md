# 🧪 Testing - Vault

## 🔐 What is Vault?

Vault is a tool that helps you store and manage sensitive information like:

- Passwords
- API keys
- Certificates
- Database credentials

![DigitalVaultNEw](https://github.com/user-attachments/assets/af27d076-4873-4594-8d50-e050d168749e)





## Before installing Vault, you need a server to run it. Here’s how to set up an Ubuntu server on AWS:

- 1) Login to AWS Console and go to EC2 Dashboard.
     
- 2) Click "Launch Instance".
  
  ![Screenshot from 2025-06-20 17-32-14](https://github.com/user-attachments/assets/161b82dc-f2ac-4194-a8a5-336b85956e85)

  
- 3) Choose AMI: Select Ubuntu Server 24.04 LTS (HVM), SSD Volume Type.
  
  ![Screenshot from 2025-06-20 17-44-38](https://github.com/user-attachments/assets/899a5b2a-f392-498c-87ba-07cd83a71bd7)


- 4) Instance Type: Choose t2.micro (free tier) or a higher type if needed.

     
- 5) Key Pair: Select or create a key pair to access your server.
  
![Screenshot from 2025-06-20 17-33-17](https://github.com/user-attachments/assets/80931634-8d27-4915-8d2e-59f88e6fa074)


- 6) Network Settings:
Allow SSH (port 22)
Allow Custom TCP on port 8200 (needed for Vault UI)

![Screenshot from 2025-06-20 17-33-58](https://github.com/user-attachments/assets/f1ab607f-976f-4cf0-a771-edd341766f8e)


- 7) Launch Instance.
     
- 8) After it’s running, connect to it using:
  
![Screenshot from 2025-06-20 17-34-47](https://github.com/user-attachments/assets/a7f7c34a-2b50-4af7-bcde-fb7587c7fd4f)

## Replace with actual public ip of instance
```
 ssh -i your-key.pem ubuntu@<your-public-ip>
```

After Connecting:

![Screenshot from 2025-06-20 17-39-44](https://github.com/user-attachments/assets/54012e0a-79c1-457e-b8a4-8dd9926fe3b3)



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

![Screenshot from 2025-06-20 17-48-37](https://github.com/user-attachments/assets/3aa6524c-4f3b-449b-bdf4-91b9f9532edd)


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


![Screenshot from 2025-06-20 17-50-33](https://github.com/user-attachments/assets/9f9ef7ae-1393-4879-998b-2597b798c22f)


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

##### 🧭 Step 1: Open Vault in Browser

Go to:


```
 http://<your-public-ip>:8200/ui
```

You’ll be redirected to the Vault initialization page.

##### 🔑 Step 2: Initialize Vault

For testing or development use:

- Key Shares: 1
- Key Threshold: 1
  
![Screenshot from 2025-06-20 17-51-55](https://github.com/user-attachments/assets/f37bdd52-f0ce-41e2-9689-845da1833902)

##### 📄 Step 3: Save Root Token and Unseal Key using download button

![Screenshot from 2025-06-20 17-52-23](https://github.com/user-attachments/assets/32c45380-5e9e-43d3-8d72-52ea29887588)

##### 🔓 Step 4: Unseal the Vault by entering the downloaded keys

Enter the Unseal Key shown in the previous step. Once the threshold is met (e.g., 1 of 1 keys), Vault will be unsealed and ready for use.

##### 🖥️ Step 5: Access the Vault Dashboard

After unsealing, you'll see the Vault Dashboard like this


![Screenshot from 2025-06-20 17-53-56](https://github.com/user-attachments/assets/38df9d1e-c15e-4dd0-8fd7-25f1e7ca08f5)


# Monitors Implemented for Testing Vault service:

# Monitors Implemented

- Health Endpoint Check:
       Verifies that the Vault health API endpoint is reachable and returning the expected status, indicating overall service health.

- Seal Status Check:
       Continuously checks whether the Vault is in a sealed or unsealed state, which is crucial for accessing.

- Token Authentication Check:
       Validates token-based authentication by sending requests using a known token to ensure that identity-based access controls are functioning correctly.

- Connectivity Check:
       Confirms network-level connectivity to the Vault server, ensuring it is accessible from the monitoring host and there are no connectivity disruptions.


