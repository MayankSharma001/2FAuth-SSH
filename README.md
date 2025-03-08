# 2FAuth-SSH

This project demonstrates how to secure SSH authentication using Google Authenticator for multi-factor authentication (MFA).

## Prerequisites
1. A Linux server
2. Root or sudo access
3. An SSH client
4. Google Authenticator app installed on your mobile device

## Step 1: Install OpenSSH Server
  - If SSH is not installed, install it using
    ```bash
      sudo apt update && sudo apt install openssh-server -y  
  - Enable and start SSH service and check if SSH is running:
   ```bash
       sudo systemctl enable ssh && sudo systemctl start ssh && sudo systemctl status sshd
```

## Step 2: Install Google Authenticator
    sudo apt install libpam-google-authenticator -y
- Run the Google Authenticator setup for your user:
  ```
    google-authenticator // Follow the prompts and save the QR code / secret key shown. Now a qr code will be generated. Scan that code with the google authenticator app and add it into your app
  ```

## Step 3: Configure SSH to Use Google Authenticator
- Edit the SSH configuration file:
  ```
  sudo nano /etc/ssh/sshd_config
- Ensure the following lines exist and are set correctly:
 ```
  - ChallengeResponseAuthentication yes
  - UsePAM yes
  - #KbdInteractiveAuthentication no // this line should either be set to "no" or make this line into a comment
  ```

## Step 4: Enable Google Authenticator in PAM
  - Edit the PAM SSH file:
      ```
       sudo nano /etc/pam.d/sshd
      ```
  - Add this line at the end then save and exit:
    ```
     auth required pam_google_authenticator.so
    ```
  - Restart SSH to apply changes:
    ```
      sudo systemctl restart sshd
    ```

## Step 5: Firewall Configuration
  ```
  sudo ufw allow 22/tcp
  sudo ufw reload
```

## Step 6: Testing SSH Login
  - Now, try logging in to your server using SSH:
    ```
     ssh username@ip  // for username enter "whoami" and for ip enter "hostname -I"
    ```
  
