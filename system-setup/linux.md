# Setup for Desktop Linux Users

If you are a Linux user, but aren’t using Ubuntu 18 LTS, you still need a way to validate that your solution will work on the instructional team's Ubuntu 18 LTS based testing infrastructure. Just as with Mac users, the suggested toolkit is Multipass, a command line tool provided by Canonical that simplifies installation of Ubuntu LTS images.

Due to the diversity of the Linux ecosystem and how these instructions differ between Ubuntu 20, Fedora, Manjaro, or whatever other distro of Linux you might be running, these instructions are kept purposely high-level. Generally, we assume that your system supports snap packages. We also assume that you possess a high degree of familiarity with Linux and are able to do sufficient troubleshooting to make this work should problems arise.

Please let the instructional team know how these instructions work for you, and consider letting us know where you hit rough spots.

_Note: This document uses the term `host` to mean your actual bare-metal Linux installation and `guest` to mean the virtualized Linux environment running in a Multipass environment named `primary`._

## Minimum Requirements

-   A Linux system with proper KVM support
-   A life philosophy that allows you to install the snap daemon

## Installation Instructions

1. Confirm that you have a working `snapd` by running `snap help` or [install snapd](https://snapcraft.io/docs/installing-snapd)
1. Open your terminal on your host environment and check if you have an SSH key on your system by seeing if `~/.ssh/id_rsa.pub` exists. If you do not have a key, you can generate one with `ssh-keygen -t rsa`. If you do not wish to set a password, just hit ENTER when asked for a passphrase. You should have a public / private keyboard located at `~/.ssh/id_rsa.pub` and `~/.ssh/id_rsa` respectively. If you generated a key, close and reopen your terminal emulator to start a new shell session. You need to do this for ssh to be able to detect the presence of this new key.
1. Generate a YAML configuration file named `primary-config.yaml` containing your public key via the following:

```bash
cd ~
echo -n -e "ssh_authorized_keys:\n  - " > ~/primary-config.yaml
cat ~/.ssh/id_rsa.pub >> ~/primary-config.yaml
```

1. Install Multipass via `sudo snap install multipass`
1. Execute the following to create a Linux guest named `primary`

```bash
cd ~
multipass launch -n primary --cloud-init primary-config.yaml 18.04
```

1. You now have a Ubuntu 18 LTS VM named primary with a default user named ubuntu. Run `multipass list` to see that this is currently running.
1. Enter the guest by running `multipass shell`
1. Set a User password. By default, Multipass does not require passwords, but some scripts require them. Run `sudo passwd ubuntu` and enter a new password
1. By default, multipass lets you access the Ubuntu shell via the command `multipass shell`. However, tools that enable remote development need to be able to use SSH. The public key you provided via the `~primary-config.yaml` file should allow this. However, before we install VSCode, we should confirm that SSH works as expected, and to do this, we need to get the IP address of your new guest instance. This IP address will change often as you start and stop your VM, so you will often need to figure out what this value is. With Multipass, there are two ways of doing this:
    1. Inside of a Multipass guest shell, you can run `ifconfig`. The address will follow `inet` and look something like this `192.xxx.xxx.xxx`
    1. In a host shell, you can run `multipass list`. The address is under the column IPv4.
1. Note the address from above, and then from a host shell, execute `ssh ubuntu@<ip_address>`, replacing <ip_address> with the address of your Multipass VM named primary.

    Depending on your distro and your SSH security settings, your system might cache fingerprints of known hosts that you've connected to in the past. These instructions may vary system to system.

    Regardless, you should see a message like the following:

    ```
    The authenticity of host '192.168.64.2 (192.168.64.2)' can't be established.
    ECDSA key fingerprint is SHA256:EpXmNgNYL6wAICuQV+YaVjeTXwbGukzYh/U328lEmvI.
    Are you sure you want to continue connecting (yes/no/[fingerprint])?
    ```

    Enter `yes`, and you should successfully log into your guest. Enter `exit` to return to your host.

    _Note: Because your guest is dynamically allocated an IP, it is possible that it might be allocated an IP address that was previously used by a different host. If this occurs, you may see a security warning such as “WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!”. If this occurs, run `vim ~/.ssh/known_hosts` and clear out the offending entry. Once this is complete, you should be able to cleanly execute `ssh ubuntu@<ip_address>`._

1. Install VSCode via `sudo snap install code --classic`
1. Install the following extensions:

    1. [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
    1. [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
    1. [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)
    1. [x86 and x86_64 Assembly](https://marketplace.visualstudio.com/items?itemName=13xforever.language-x86-64-assembly)

    via the following shell commands

    ```sh
    code --install-extension ms-vscode-remote.remote-ssh
    code --install-extension ms-vscode.cpptools
    code --install-extension MS-vsliveshare.vsliveshare-pack
    code --install-extension 13xforever.language-x86-64-assembly
    ```

    Optionally, if you are coming from vim, emacs, or some other editor, and have a working muscle memory for those keybindings, [consider installing a keymap extension](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Relevance).

1. Launch VSCode and confirm that the extensions have installed properly. You will likely see installation messages on first start, including a message prompting you to enter your user password to to install additional dependencies that the extensions require. Once this is complete, open the VSCode Extensions panel. You should see the extensions listed located under `LOCAL - INSTALLED`. If one of these extensions shows a `Reload Required` button, click it and wait for VSCode to reload.
1. Now that VSCode is configured, let's connect it to our Multipass VM. To do this, click on the green button in the bottom left corner. This should open a menu listing several `Remote-SSH` options. Click `Remote-SSH: Connect to Host...`. Enter `ubuntu@<ip_address>`, replacing <ip_address> with the address of your Multipass VM as before. This will open a new VSCode window. Feel free to close the old window. After a brief installation of the VSCode backend onto your VM, the bottom left corner's green button should now read: `SSH: <ip_address>`.
1. Hit ctrl+\` in VSCode, and ensure that the terminal in VSCode opens to your Linux home directory. It should show `ubuntu@primary:~$`
1. By default, the previous extensions you installed are running on the host operating system. However, some of these need to run on the remote backend running on your Linux guest. To address this, open the VSCode Extensions panel, and look under the heading `LOCAL - INSTALLED`. You will see several extensions that are grayed out and have a button `Install in SSH: <ip_address>`. Click this button for each extension.
1. You should now see these extensions under `WSL:UBUNTU-18.04 - INSTALLED`. However, they likely have a button `Reload Required`. Once all extensions are installed under Ubuntu, click that button to reload the window. As before, you will likely be prompted to install additional Linux dependencies, including prompting for your user password. Remember that this is the password you set for the ubuntu user on the guest operating system, not the user on your host system. When complete, you will be asked to reload VSCode yet again. Click “Reload Now.”
1. Hit ctrl+\` to open the integrated shell in VSCode. Visually check that this is your guest (username ubuntu) and not your host environment. We're now ready to install xv6 and its dependencies.

```sh
sudo apt-get update --yes
sudo apt-get install build-essential gdb qemu-system-x86 --yes
cd ~
mkdir projects
cd projects
git clone https://github.com/gwu-cs-os/gwu-xv6
cd gwu-xv6
make qemu-nox
```

1. Assuming everything completed properly, you should boot into xv6, an operating system modeled after version 6 of Bell Labs Research Unix.
1. In xv6, run `cat README`
1. Exit xv6 by hitting ctrl+a (then releasing both keys) and then x.
