# Using SSH Keys with Passwords

When you create an SSH key, you are allowed to assign a password to it.  This means whenever you attempt to use the key it will prompt you with a password.  Some software, such as VS Code, can deal with this for you and cache the appropriate permissions it needs.

Other software (for example the command line version of git) will always prompt for the password associated with the SSH key being used.  This can be rather annoying.  It is possible to cache the permissions such that you only need to enter the password at the start of a session.

This is done using a tool called **ssh-agent**.  It is available on both the Windows and WSL environments.  This document will focus primarily on the WSL environment.

## SSH-Agent Under WSL

### Overview

The ssh-agent program will allow a password for a key to be cached for automatic reuse.  This means we need to perform the following tasks

- Make sure that ssh-agent is running
- Add the key to ssh-agent

### Procedure

To use ssh-agent to cache your key password, do the following

1. Start a WSL shell.  Make sure you are in your home directory by running this command

   ```text
   cd ~
   ```

2. Amend your .bashrc script to load the ssh-agent program when you login to a WSL shell.  This can be accomplished by using a text editor.  There are several text editors you could use - vi, nano or VS Code.  Once you are editing the .bashrc script, then add the following to the end of the script:

   ```text
   if ! [ -d $HOME/.ssh/ ]; then
     mkdir $HOME/.ssh/
   fi

   # Is there an instance running already in the shell
   if [ -z "$SSH_AUTH_SOCK" ]; then
     # There is not, so check for a currently running instance of the agent from a different session
     RUNNING_AGENT="`ps -ax | grep 'ssh-agent -s' | grep -v grep | wc -l | tr -d '[:space:]'`"

     if [ "$RUNNING_AGENT" = "0" ]; then
       # Launch a new instance of the agent
       ssh-agent -s > $HOME/.ssh/ssh-agent
     fi

     # Make sure the ssh-agent environment variables are set
     eval `cat $HOME/.ssh/ssh-agent`
   fi
   ```

   The purpose of these changes is to ensure that the ssh-agent is running, and is only started once.  Any subsequent logins will use the same instance of the ssh-agent program.

3. Next we need to ensure that the key is available for the ssh-agent to use.  We do this by using a command called ssh-add.  The following command will accomplish this.

   ```text
   ssh-add ~/.ssh/id_rsa.pub
   ```

   Note that if the ssh key being added, has a password, then it will prompt for that password this time.  Subsequently when the key is used it will not need to prompt you.

4. We can check that the key was added successfully by using this command.  If the key was added correctly, it will list the key fingerprint.

   ```text
   ssh-add -l
   ```

## SSH-Agent Under Windows

The basic procedure is very similar to the WSL environment.  You need to

1. Start the ssh-agent.  The ssh-agent program is implemented as a Windows Service and may need to be installed.  Do this by running the following command from an Administrative Powershell window.

   ```text
   Get-Service ssh-agent | Set-Service -StartupType Automatic -PassThru | Start-Service
   ```

2. Add a key using ssh-add.

   ```text
   ssh-add c:\users\example\.ssh\id_rsa
   ```

3. To check that the key has been added, simply use the ssh-add key as follows

   ```text
   ssh-add -l
   ```

For a more detailed discussion of this process please refer to this article [SSH Keys on Windows 10](https://richardballard.co.uk/ssh-keys-on-windows-10/)
