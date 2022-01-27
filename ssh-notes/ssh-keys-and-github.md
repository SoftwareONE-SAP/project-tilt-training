# SSH Keys

When setting up Git Hub to work with VS Code there are a couple of potential issues you may encounter.  These are fairly simple to resolve.

We will assume that you have a WSL 'machine' set up in Windows.

## Checking for an SSH Key

Before you can work with any of the Centiq repo's on Git Hub, you will need to ensure that you have an SSH key set up.

If you are not sure if you have an SSH key,

- Start a WSL shell and connect to your WSL installation. (A simple way to do this is to use 'Start Menu>Run>WSL' in Windows).

- Then look in the folder '.ssh' in your home directory.  To do this use the following commands in the shell.

```bash
cd ~
cd .ssh
ls -al
```

If you have an SSH key then you will see that there will be two files:

```bash
id_rsa
id_rsa.pub
```

These are the private key (id_rsa) and public key (is_rsa.pub) files.  You need to ensure that they are key safe so it is worth making a copy of them in a secure location.

## To Create An SSH Key

If, having followed the steps in the previous section, you find you do not have an SSH key, then you can create one.
Use this command from the WSL shell:

```bash
ssh-keygen -t rsa -b 4096
```

Follow the prompts and once it has finished then it will produce the two id_rsa files we were looking for previously.

## To Upload the SSH Key to Git Hub

Once you have located the SSH key files, then you need to associate them with your Git Hub account.  You can do this as follows-

1. Log into [Github](http://github.com) in a browser.
2. Click on your profile picture (top right hand corner), and choose 'Settings'.
3. In the left hand menu select 'SSH and GPG Keys'.
4. On the page that is shown, press the button 'New SSH Key'.
5. Give the key a descriptive title.
6. Paste in the entire contents of the id_rsa.pub file, including any comments.
7. Press 'Add SSH Key'

Once you are done the key should be listed.

## Test Git Hub SSH Connection

Once you have a key uploaded to the Git Hub then you can verify that the connection is working and that your key is recognised.

1. Start a WSL shell.
2. Use the following command -

```bash
ssh -T git@github.com
```

You should see a message telling you the public fingerprint of the server you are connecting to, and, if your key was password protected, it may prompt you for the password of your SSH key.
Here is a some typical output from the command:

```text
The authenticity of host 'github.com (140.82.121.4)' can't be established.
ECDSA key fingerprint is SHA256:ABCDEFGHIJKLMNOPQRSTUVWXYZ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.121.4' (ECDSA) to the list of known hosts.
Enter passphrase for key '~/EXAMPLEUSER/id_rsa':
Hi EXAMPLEUSER! You've successfully authenticated, but GitHub does not provide shell access.

```

If you have problems with the connection test appearing to hang when it is performed see [ssh-connection-problems](ssh-connection-problems.md) for remediation.
