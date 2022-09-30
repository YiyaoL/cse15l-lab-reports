# Lab Report Week 1: Remote Access
*This report will guide you through the process of accessing a remote server.*

### 1. Get Your Account
You will first need to look up your student account for the course. 
- Go to [this website](https://sdacs.ucsd.edu/~icc/index.php) and type in your UCSD username and student id. Then click "submit". 
- This will be your account name.
![step1-2](look_up2.png)

### 2. Install Visual Studio Code
If you haven't already, download VS Code from [this website]( https://code.visualstudio.com/) and follow the installation instructions on your laptop. 
- When you open it, something like this will show up:
![step2](vs.png)
- Open a terminal. You can open a new terminal in VS Code by clicking Ctrl+shift+` or using Terminal -> New terminal in the top menu. You can also use the terminal in your laptop. 

### 3. Establish Remote Access
We will use the command `ssh` (secure shell) for the remote access.
- Open up a terminal, type the following command, with the "x"s being replaced by your account name:
```
$ ssh xxxxxxxxxxx@ieng6.ucsd.edu
```
- Then you will need to type in your password. The password you type will *not* show up on the screen. You can first type it elsewhere and paste it to prevent typos.
- The first time you connect, there may be a message to ask you if you want to continue connecting (I forgot to take a screenshot in lab of that message). Type "yes".
- Then you might see something like:
![step3_1](ssh_1.png)
Congratulations! This means you are in. 

- If you are prompted to type the password multiple times and are still unable to log in, try changing your **course specific (NOT your tritonlink account)** password [here](https://sdacs.ucsd.edu/~icc/password.php). 

### 4. Running Commands
Try out some basic commands. 
- `cd`: change directory ; `cd ~`: change to home directory ; `ls`: list files ; `cat`: print out content ; `ls-lat`: list files with full information
![step4_1](cmd_1.png)
![step4_2](cmd_2.png)
- Try to change directory to another account. You should be able to access the public account, but not the account of another student. 
![step4_3](cmd_3.png)

### 5. Create a Secure Copy 
- Log out of ssh using `exit`. 
- Create the following WhereAmI.java file in VS Code and run it on your own laptop.
    ```
    class WhereAmI {
        public static void main(String[] args) {
            System.out.println(System.getProperty("os.name"));
            System.out.println(System.getProperty("user.name"));
            System.out.println(System.getProperty("user.home"));
            System.out.println(System.getProperty("user.dir"));
        }
      }
    ```
    This will print out the name of your operating system, your user name, home directory, and working directory. For example: 
    ![step5_1](wai_1.png)
    
    *(This screenshot is from my laptop cmd. You can also run it either through VS code.)*

- Now, move the file from your laptop to the server using the `scp` (secure copy) command. Then, log into your remote access account. 
![step5_2](move.png)

- Run the file again and you will see something like this. Notice that the system, name, and directories change. 

![step5_3](wai_2.png)

### 6. Set an SSH Key
Switching between accounts can be very repetitive, so we will set an ssh key to solve this problem. The idea is to generate a public-private key pair. Store the public key in the server and the private key in your laptop. The next time you need to switch between server and client, ssh will use the key pair instead of asking for your password. 
- On your laptop, use the `ssh-keygen` command to create keys. Press `Enter` to save the key at the default path. Then enter a passphrase (or press `Enter` for no passphrase). 
![step6_1](key_1.png)
- If you are on windows, your ssh-agent(which saves the key) is disabled by default. Follow the steps from [this link](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation) or [this link](https://stackoverflow.com/questions/52113738/starting-ssh-agent-on-windows-10-fails-unable-to-start-ssh-agent-service-erro) to enable your ssh-agent.
- Next, add your key using the `ssh-add` command. 
![step6_2](add_1.png)
- Copy the public key to your server using `scp`, and you should be able to access the remote server without typing the password. 

**_However, I keep getting `no such file or directory`._**
    
(1) Check if id_rsa.pub exists in my laptop:

![step6_3](scp_1.png)

(2) Try to copy to server:

![step6_4](scp_2.png)
