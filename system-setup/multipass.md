# Setup for Mac and Linux Users

The suggested toolkit for Mac and Linux users is Multipass, a command line tool provided by Canonical that simplifies installation of Ubuntu LTS images.

If you are running Ubuntu 20 LTS as your host environment and have stock compiler versions, you may run the xv6 environment natively. In this case, skip installation of Multipass, SSH key generation, everything in _Setup your Multipass VM_, and everything is _Connect VSCode to your Multipass VM_, resuming with the steps in _Setup xv6_

## Minimum Requirement

### Mac

-   Mac OS Yosemite, version 10.10.3 or later installed on a 2010 or newer Mac.
-   Unfortunately, we don't know how currently to get x86 Ubuntu running on OSX with M1 chips.
    Hop into `#tech-support` and discuss.
	You might use [UTM or QEmu](https://nomadic-dmitry.medium.com/apple-silicon-m1-how-to-run-x86-and-arm-virtual-machines-on-it-cdd9d9054483) and perhaps [docker](https://docs.docker.com/desktop/mac/apple-silicon/).
	If you get this working, please let us know and do a PR with instructions!

### Linux

-   KVM and the ability to support snapd

Note that if you're already running Ubuntu, there is no reason to do anything more!
Just use your own system.

## Installation Instructions

If you want to use your own technique to install a VM with Ubuntu 20 LTS, feel free to.
If want a guided means of doing so, see below.
Note, we're still looking for a way to do this with Apple M1 chips.

### Setup Dependencies on your Host Machine

1. Confirm that your package manager is setup
    - For Mac pre-M1 chips, check that brew is installed by running `brew help` or [install it](https://brew.sh/)
    - For Linux, check that snap is install by by running `snap help` or [install it](https://snapcraft.io/docs/installing-snapd)
2. Install Multipass
    - Mac:
    ```sh
    brew install --cask multipass
    ```
    - Linux:
    ```sh
    sudo snap install multipass
    ```
3. Open Terminal and check if you have an SSH key on your system by seeing if `~/.ssh/id_rsa.pub` exists. If you do not have a key, you can generate one with `ssh-keygen -t rsa`. If you do not wish to set a password, just hit ENTER when asked for a passphrase. You should not have a public / private keyboard located at `~/.ssh/id_rsa.pub` and `~/.ssh/id_rsa` respectively. If you generated a key, close and reopen Terminal to start a new shell session. You need to do this for ssh to be able to detect the presence of this new key.

### Setup your Multipass VM

4. Generate a YAML configuration file containing your public key via the following:

    ```sh
    cd ~
    echo -n -e "ssh_authorized_keys:\n  - " > ~/primary-config.yaml
    cat ~/.ssh/id_rsa.pub >> ~/primary-config.yaml
    ```

5. Create a Linux guest named `primary`. If you are on a system with more than 8GB of RAM, you may optionally want to increase the memory allocated to this VM from 1.5GB to 2GB by changing `-m 1500M` to `-m 2G` below.

    ```bash
    cd ~
    multipass launch -n primary -c 4 -m 1500M --cloud-init primary-config.yaml 20.04
    ```

6. You now have a Ubuntu 20 LTS VM named primary with a default user named ubuntu. Run `multipass list` to see that this is currently running and that it is the only VM listed.
10. Enter the guest by running `multipass shell`
11. Confirm that your VM has multiple virtual processors by running `lscpu`. Among others things, you should see:
    ```
    CPU(s):              4
    On-line CPU(s) list: 0-3
    Thread(s) per core:  2
    Core(s) per socket:  2
    Socket(s):           1
    NUMA node(s):        1
    ```
7. Set a User password. By default, Multipass does not require passwords, but some scripts require them. Run `sudo passwd ubuntu` and enter a new password
8. By default, multipass lets you access the Ubuntu shell via the command `multipass shell`. However, tools that enable remote development need to be able to use SSH. The public key you provided via the `~primary-config.yaml` file should allow this. However, before we install VSCode, we should confirm that SSH works as expected, and to do this, we need to get the IP address of the VM. This IP address will change often as you start and stop your VM, so you will often need to figure out what this value is. With Multipass, there are two ways of doing this:
    - In the primary guest, you can run `ifconfig`. The address will follow `inet` and look something like this `192.xxx.xxx.xxx`
    - In the host OS, you can run `multipass list`. The address is under the column IPv4.
9. Note the address from above, and then from a host shell, execute `ssh ubuntu@<ip_address>`, replacing <ip_address> with the address of your Multipass VM named primary. You should see a message like the following:

    Depending on your distro and your SSH security settings, your system might cache fingerprints of known hosts that you've connected to in the past. This likely resembles a message like the following:

    ```
    The authenticity of host '192.168.64.2 (192.168.64.2)' can't be established.
    ECDSA key fingerprint is SHA256:EpXmNgNYL6wAICuQV+YaVjeTXwbGukzYh/U328lEmvI.
    Are you sure you want to continue connecting (yes/no/[fingerprint])?
    ```

    Enter `yes`, and you should successfully log into your guest. Enter `exit` to return to your host.

    _Note: Because your guest is dynamically allocated an IP, it is possible that it might be allocated an IP address that was previously used by a different host. If this occurs, you may see a security warning such as “WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!”. If this occurs, run `vim ~/.ssh/known_hosts` and clear out the offending entry. Once this is complete, you should be able to cleanly execute `ssh ubuntu@<ip_address>`._

### Setup xv6

10. Open a shell in your VM (e.g. using ssh or in VSCode if it is not already open and hit ctrl+\` to open the integrated shell). It should show `ubuntu@primary:~$`. We're now ready to install xv6 and its dependencies.

    ```sh
    sudo apt-get update --yes
    sudo apt-get install build-essential gdb glibc-doc qemu-system-x86 --yes
    cd ~
    mkdir projects
    cd projects
    git clone https://github.com/gwu-cs-os/gwu-xv6
    cd gwu-xv6
    code -r .
    ```

11. This will open the xv6 project. Feel free to look at the source code. Reopen the terminal and build xv6.

    ```sh
    make qemu-nox
    ```

12. Assuming everything completed properly, you should boot into xv6, an operating system modeled after version 6 of Bell Labs Research Unix.
13. In xv6, run `cat README`
14. Exit xv6 by hitting ctrl+a (then releasing both keys) and then x.

## Appendix: Setting up VSCode (**optional**)

You can optionally use VSCode.
If you want to, see below some options.

1. Install VSCode (**optional**)
    - Mac:
    ```sh
    brew cask install visual-studio-code
    ```
    - Linux:
    ```sh
      sudo snap install code --classic
    ```
2. Install the following extensions:

    - [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
    - [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
    - [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)
    - [x86 and x86_64 Assembly](https://marketplace.visualstudio.com/items?itemName=13xforever.language-x86-64-assembly)

    via the following shell commands

    ```sh
    code --install-extension ms-vscode-remote.remote-ssh
    code --install-extension ms-vscode.cpptools
    code --install-extension MS-vsliveshare.vsliveshare-pack
    code --install-extension 13xforever.language-x86-64-assembly
    ```

    Optionally, if you are coming from vim, emacs, or some other editor, and have a working muscle memory for those keybindings, [consider installing a keymap extension](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Relevance).

3. Launch VSCode and confirm that the extensions have installed properly. You will potentially see installation messages on first start, including possibly a message prompting you to enter your user password to to install additional dependencies that the extensions require. Once this is complete, open the VSCode Extensions panel. You should see the extensions listed located under `LOCAL - INSTALLED`. If one of these extensions shows a `Reload Required` button, click it and wait for VSCode to reload.

### Connect VSCode to your Multipass VM

15. Open VSCode and click on the green button in the bottom left corner. This should open a menu listing several `Remote-SSH` options. Click `Remote-SSH: Connect to Host...`. Enter `ubuntu@<ip_address>`, replacing <ip_address> with the address of your Multipass VM as before. This will open a new VSCode window. Feel free to close the old window. After a brief installation of the VSCode backend onto your VM, the bottom left corner's green button should now read: `SSH: <ip_address>`.
16. Hit ctrl+\` in VSCode, and ensure that the terminal in VSCode opens to your Linux home directory. It should show `ubuntu@primary:~$`
17. By default, the previous extensions you installed are running on the host operating system. However, some of these need to run on the remote backend running on your Linux guest. To address this, open the VSCode Extensions panel, and look under the heading `LOCAL - INSTALLED`. You will see several extensions that are grayed out and have a button `Install in SSH: <ip_address>`. Click this button for each extension.
18. You should now see these extensions under `SSH: <ip_address> - INSTALLED`. However, they likely have a button `Reload Required`. Once all extensions are installed under Ubuntu, click that button to reload the window. You will be prompted to install additional Linux dependencies, including prompting for your user password. When complete, you will be asked to reload VSCode yet again. Click “Reload Now.”


## Appendix: Deleting and Rebuilding the VM

Currently, multipass does not provide an easy mechanism to modify the number of virtual processors and memory allocated to your virtual machine. As a result, if you underprovision the amount of memory assigned to your VM, your best bet is to delete, purge, and rebuild your primary VM. These instructions are intended to take you through that process. Please note that this deletes all data on this VM!

1. Log into you multipass VM and backup data that you are concerned with losing. Generally, this should only be assignments and labs, which should always be saved, committed and pushed to GitHub. If there is any data that you need to save, you can back this data up by copying the files/directories to your host filesystem. By default, multipass mounts your host machine's home folder at `~/Home`.
2. Run `multipass delete primary` to delete your VM. This is recoverable and equivalent to placing the VM in your "Trash Can" on the desktop.
3. Run `multipass purge` to purge the data and "empty the Trash Can."
4. Run `multipass list` to confirm that your instances are purged
5. At this point, you should repeat the original setup instructions from the `Setup your Multipass VM` section onwards. *Be sure to actually make the changes to the launch command that you care about!* For the VSCode steps, you will not need to reinstall VSCode itself, but you will need to reinstall the remote VSCode backend and plugins on your Linux VM.

## Appendix: Troubleshooting

_Note: If you hit a snag, first try restarting your machine before proceeding._

The following script can be used to generate useful information to help the instructional staff debug issues:

```sh
#!/bin/sh

main() {
  echo "Mac Hardware:"
  system_profiler SPHardwareDataType

  echo "Multipass Info:"
  multipass info --all

  echo "Primary lscpu:"
  multipass exec primary -- lscpu

  echo "Primary lsmem:"
  multipass exec primary -- lsmem
}

main "$@"
```

## Appendix: Using A Permanent Alias For Ease of Access

_Note: This is purely to make running xv6 easier from the terminal and is in no way needed for xv6 to work._

The following steps can be used to create a permanent alias that fires off all the commands needed to run xv6 easily.

**1. Figure out your commands!**

To start, we want to get together all the commands we need to run xv6.
These include navigating to where we cloned the provided repo, opening the xv6 project in VSCode, and then finally building xv6.
The commands needed to do so are:

```
cd ~/projects/gwu-xv6
code -r .
make qemu-nox
```

_Note: The location you cloned the provided repo is subject to change, so make sure you have said location in the first command above._

**2. Create your alias!**

Once you have these commands ready (try running them manually to verify they are correct), it is time to make our alias command!
A generic alias command is seen below:

```
alias proj="cd /home/tree/projects/java"
```

Above we can see that the alias "proj" will fire off the command "cd /home/tree/projects/java" when "proj" is run as a command in the terminal.
Using this command as our template, we can craft our own alias using the previously gathered commands, separating individual commands with "; ":

```
alias xv6='cd ~/projects/gwu-xv6; code -r .; make qemu-nox'
```

**3. Find where to put your alias!**

Now, we could stop right here and just simply run this alias command in our terminal.
The only issue is that this alias will exist until you kill the terminal session, then it will be gone.
To make our alias permanent, we need to put it in the ~/.bash_aliases file.
This file is loaded by "~/.bashrc", and we can't just simply open it, we have to source it.
To source and edit it, we can simply use our favorite text editor, VIM!

```
vim ~/.bashrc
```

Once we have done this, we will be able to edit the file.
Search for the following lines in the file and make sure they are not commented.

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

**4. Insert your alias!**

Once uncommented, if they weren't already, we want to insert our newly made alias command below the above lines as so:

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

alias xv6='cd ~/projects/gwu-xv6; code -r .; make qemu-nox'
```

Once done, :wq your way out of there!

**5. Test your alias!**

Enjoy your new permanent alias by running your alias as a command!

```
xv6
```

**Enjoy!**
