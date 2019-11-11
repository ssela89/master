# Changing SSH Access after BTCPay installation over VPS (lunanode)

The following tutorial explains how to disable password login over ssh for your BTCPay server and create an alternative. This is recommended for security reasons. In this example we assume you are a windows user and use PuTTY to generate our private-public key pair. If you are a linux user I assume you will find a way to generate your keys. After you generated the key you can also use the web-interface to save the public key on the server (step 5). 
1. Download PuTTY form a secure server and install it.
2. Open PuTTYgen. Make sure RSA is selected. Hit “Generate”.
3. Choose a phassphrase (password) that protects your key and write it down.
4. Save the private key in a secure location. 
5. Log into your BTCPay server admin account and navigate to server settings -> services -> ssh -> see information. 
6. Most probably you will already see an ssh-rsa key under “authorized keys”. This is used by BTCPay for updates and maintenance. Don’t change it.
7. Go Back to PuTTYgen and copy your public key. Go Back to the BTCPay webinterface, click into the “authorized keys field”, hit “enter” and paste the public key you just created right below the first key and click save.
8. Try to log-in using your new private-public key pair: Open PuTTY and under hostname type the IP address of your server (visible in lunanode dashboard). Then use the mouse to navigate to SSH and click on “auth”. Use the Browse field to find your saved private key file (step 4) and click “open”. As user type “root” and if you set a phassphrase (step 3) you will have to type it as well. 
9. If your ssh connection was successfull type ```nano /etc/ssh/sshd_config``` . Scroll down to find “PasswordAuthentication yes” and replace yes with “no”. Exit and save. (Note: This means from now on you will **NOT** be able to log in using a user & password combination.)
10. To test the settings restart the service (type ```sudo service ssh restart```) or simply restart your whole server with ```sudo reboot```. (note: your ssh-session will terminate whe using the second option)
11. After a few minutes try to reconnect using ssh using the user + password combination. Once you type your username you should get a message similar to “Permission denied (publickey).” 

That’s it. Your ssh access is now save from brute force attacks. Make sure you store your private key in a save place and backup it somewhere. You can now use your new ssh access to regularly check for updates of your VMs OS.
