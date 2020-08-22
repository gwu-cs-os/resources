# Setup for Windows 10 Users

The suggested toolkit for Windows 10 users is Windows Subsystem for Linux versions 2 (WSL 2).

This environment runs a headless Ubuntu virtual machine in a lightweight virtual machine, with various interop features with the host Windows 10 environment.

## Minimum Requirement

-   Windows 10, version 2004, Build 19041 or higher. Check this by typing `Windows Logo Key` + R, typing `winver` in the Run window, and selecting OK.

## Installation Instructions

1. Install the new Windows Terminal. This generally does a better job with Linux shells, and it makes it easier to toggle between PowerShell, DOS, and (once installed) your Ubuntu 18 guest. https://www.microsoft.com/store/productId/9N0DX20HK701
1. Install the WSL 2 environment. https://docs.microsoft.com/en-us/windows/wsl/install-win10. When you get to the _Install your Linux distribution of choice_ step, select Ubuntu 18.04 LTS

Note: If you see `Error: 0x1bc` or a message similar to `WSL 2 requires an update to its kernel component. For information please visit https://aka.ms/wsl2kernel` when you run `wsl --set-default-version 2`, download and install the kernel update from https://docs.microsoft.com/nl-nl/windows/wsl/wsl2-kernel

1. Verify that your Linux guest is using version 2 by running `wsl --list --verbose` in a PowerShell session
1. Install VSCode and complete this tutorial https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode up to _Extensions inside of VS Code Remote_
1. Close VSCode and open a Linux shell in Windows Terminal. Type `code .` in your home directory and ensure that you are able to launch VSCode with an open Linux directory.
1. Hit ctrl+` in VSCode, and ensure that the terminal in VSCode opens to your Linux home directory.
1. Install the following extensions:

    1. [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    1. [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
    1. [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)
    1. [x86 and x86_64 Assembly](https://marketplace.visualstudio.com/items?itemName=13xforever.language-x86-64-assembly)

    To save time, you can install via the following commands in PowerShell

    ```sh
    code --install-extension ms-vscode-remote.remote-wsl
    code --install-extension ms-vscode.cpptools
    code --install-extension MS-vsliveshare.vsliveshare-pack
    code --install-extension 13xforever.language-x86-64-assembly
    ```

    Optionally, if you are coming from vim, emacs, or some other editor, and have a working muscle memory for those keybindings, [consider installing a keymap extension](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Relevance).

1. By default, these extensions were installed on your host (Windows) operating system. However, some of these need to run on your Linux guest. To address this, open the VSCode Extensions panel (Ctrl+Shift+X), and look under the heading `LOCAL - INSTALLED`. You will see several extensions that are grayed out and have a button `Install in WSL:Ubuntu:18.04`. Click this for each.
1. You should now see these extensions under `WSL:UBUNTU-18.04 - INSTALLED`. However, they likely have a button `Reload Required`. Once all extensions are installed under Ubuntu, click that button. You may be prompted to enter your Linux password to install additional backend Linux dependencies
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
