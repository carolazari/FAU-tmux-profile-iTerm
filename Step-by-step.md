#This is a quick guide to be able to set up an iTerm profile. 
#In the past (and maybe recommended) I have followed Eric Anderson's tutorial https://www.youtube.com/watch?v=vL6swuC6IMU.
#However, this approach did not work for me this time around, so I tried something new
#There are a couple of things happening here, and some of them are in your LOCAL Mac and some others on your remote.

#Step 1: Configure Your Local SSH Config File (Local Mac Setup)Instead of forcing iTerm2 to parse complex strings in its graphical settings, we offload the path mapping directly #to your Mac's native SSH utility.Open a terminal on your local Mac (not on the cluster).Open or create your SSH configuration file by running:

```
nano ~/.ssh/config
```
#Paste this block into your local Mac's SSH config file (~/.ssh/config):

```

Host fau-tmux
    # The domain name or IP address of the remote host
    HostName athene-login.hpc.fau.edu
    
    # Replace with your actual cluster login username (e.g., your FAU NetID)
    User <YOUR_USERNAME_HERE>
    
    # Forces SSH to allocate a pseudo-terminal (equivalent to the -t flag)
    RequestTTY yes
    
    # Automatically activates the Conda tmux environment and launches Control Mode
    # 1. Swaps <YOUR_CONDA_ENV_NAME> with your explicit environment name
    # 2. Swaps <YOUR_SESSION_NAME> with your preferred tmux session identifier
    RemoteCommand export LD_LIBRARY_PATH=$HOME/miniconda3/envs/<YOUR_CONDA_ENV_NAME>/lib:$LD_LIBRARY_PATH; $HOME/miniconda3/envs/<YOUR_CONDA_ENV_NAME>/bin/tmux -CC attach -t <YOUR_SESSION_NAME> || $HOME/miniconda3/envs/<YOUR_CONDA_ENV_NAME>/bin/tmux -CC new -s <YOUR_SESSION_NAME>


```
# As an example, this is how my ~/.ssh/config looks like

```
Host fau-tmux
    HostName athene-login.hpc.fau.edu
    User my-FAUNetID
    RequestTTY yes
    RemoteCommand export LD_LIBRARY_PATH=$HOME/miniconda3/envs/tmux/lib:$LD_LIBRARY_PATH; $HOME/miniconda3/envs/tmux/bin/tmux -CC attach -t item || $HOME/miniconda3/envs/tmux/bin/tmux -CC new -s item

```
#Step 2: Create the iTerm2 ProfileNow, tie the clean SSH shortcut straight into a dedicated iTerm2 profile GUI.Open iTerm2 on your Mac.Go to the top menu bar and navigate to #Settings (or Preferences) > Profiles.Click the + icon at the bottom of the profile list to create a new profile. Name it something descriptive like Athene HPC (Tmux).Under the #General tab, look for the Command section:Change the dropdown menu from Login Shell to Command.In the text box, type exactly:

```
ssh <CHOOSE_A_NICKNAME>
#example fau-tmux
```

#Step 3: Save Your Password in iTerm2Instead of typing your cluster password every time, you can save it securely inside iTerm2's built-in Password Manager.
#Open iTerm2 and go to the top menu bar > Window > Password Manager.
#Click the + icon to add a new password entry.
#Enter your account password and give it a clear label (e.g., FAU Cluster Password). 
#Click OK and close the manager.
#Go back to your profile settings (Settings > Profiles > select your profile).
#Under the General tab, look below the Command field for Advanced Settings.
#Find the Triggers section and click Edit.
#Create a new trigger with these exact settings to automate the popup:
#Regular Expression: Password:
#Action: Open Password Manager
#Parameters: Select your saved password label from the dropdown list.
#Check the Instant box on the far right so it fires immediately when the cluster asks for your password.

#How to Use It:
How to Use ItClick Profiles in the iTerm2 menu bar and select your new profile.
#iTerm2 will connect and automatically pop open the Password Manager window.
#Simply click your saved cluster password or hit Enter to submit it.Approve your Multi-Factor Authentication prompt (e.g., Duo Push).
#The terminal will immediately hand control over to iTerm2, and your remote cluster session will seamlessly populate as native, draggable Mac tabs!
