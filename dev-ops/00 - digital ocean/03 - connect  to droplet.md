### Connecting SSH with Digital Ocean

Connecting your local machine to a remote droplet using SSH allows you to manage your server without the need for a password. This is achieved by using SSH keys, which provide a secure and convenient method for authentication.

Copying the public key to the remote server makes passwordless authentication possible because it sets up a trust relationship between your local machine and the server. 

When you try to log in to the server, the server uses the public key stored in the `~/.ssh/authorized_keys` file to verify the private key on your local machine. 

If the keys match, authentication is successful without the need for a password, allowing for a secure and seamless login process. This method relies on cryptographic techniques that ensure only the holder of the corresponding private key can access the server, thereby enhancing security.

### Connecting on macOS

1. **Open Terminal**:
   On your macOS, open the Terminal application.

2. **Add the SSH key to your SSH agent**:
   Ensure your SSH key is loaded into the SSH agent. If you followed the previous steps, you can add it using:
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/newproject_rsa
   Replace "newproject" with the first word of your project.

3. **Connect to your droplet**:
   Use the SSH command to connect to your droplet. Replace `your_droplet_ip` with the actual IP address of your droplet.
   ssh root@your_droplet_ip
   This command will use the SSH key added to your SSH agent for authentication.

4. **Verify the connection**:
   You should now be connected to your droplet without being prompted for a password. You will see the command prompt change to reflect that you are now operating on the remote server.

   You can disconnect from the server by typing `exit` and press `return key`.

### Connecting on Windows

1. **Open Command Prompt or PowerShell**:
   On your Windows machine, open Command Prompt or PowerShell.

2. **Add the SSH key to your SSH agent**:
   Ensure your SSH key is loaded into the SSH agent. If you followed the previous steps, you can add it using:
   eval "$(ssh-agent -s)"
   ssh-add C:\Users\your_username\.ssh\newproject_rsa
   Replace "newproject" with the first word of your project.

3. **Connect to your droplet**:
   Use the SSH command to connect to your droplet. Replace `your_droplet_ip` with the actual IP address of your droplet.
   ssh root@your_droplet_ip
   This command will use the SSH key added to your SSH agent for authentication.

4. **Verify the connection**:
   You should now be connected to your droplet without being prompted for a password. You will see the command prompt change to reflect that you are now operating on the remote server.

   You can disconnect from the server by typing `exit` and press `return key`.

By following these detailed steps, you can securely connect to your Digital Ocean droplet from both macOS and Windows machines, leveraging SSH keys for passwordless authentication. This setup enhances security and convenience, allowing you to efficiently manage your remote servers.
