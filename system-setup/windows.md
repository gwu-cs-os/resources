# Setup for Windows 10 Users

The suggested toolkit for Windows 10 users is Windows Subsystem for Linux versions 2 (WSL 2).

This environment runs a headless Ubuntu virtual machine in a lightweight virtual machine, with various interop features with the host Windows 10 environment.

## Minimum Requirement

-   Windows 10, version 2004, Build 19041 or higher. Check this by typing `Windows Logo Key` + R, typing `winver` in the Run window, and selecting OK.

## Installation Instructions

1. Install the new Windows Terminal. This generally does a better job with Linux shells, and it makes it easier to toggle between PowerShell, DOS, and (once installed) your Ubuntu 18 guest. https://www.microsoft.com/store/productId/9N0DX20HK701
2. Install the WSL 2 environment. https://docs.microsoft.com/en-us/windows/wsl/install-win10. When you get to the _Install your Linux distribution of choice_ step, select Ubuntu 18.04 LTS

    Note: If you see `Error: 0x1bc` or a message similar to `WSL 2 requires an update to its kernel component. For information please visit https://aka.ms/wsl2kernel` when you run `wsl --set-default-version 2`, download and install the kernel update from https://docs.microsoft.com/nl-nl/windows/wsl/wsl2-kernel

3. Verify that your Linux guest is using version 2 by running `wsl --list --verbose` in a PowerShell session
4. Install VSCode and complete this tutorial https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode up to _Extensions inside of VS Code Remote_
5. Close VSCode and open a Linux shell in Windows Terminal. Type `code .` in your home directory and ensure that you are able to launch VSCode with an open Linux directory.
6. Hit ctrl+` in VSCode, and ensure that the terminal in VSCode opens to your Linux home directory.
7. Install the following extensions:

    - [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    - [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
    - [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)
    - [x86 and x86_64 Assembly](https://marketplace.visualstudio.com/items?itemName=13xforever.language-x86-64-assembly)

    To save time, you can install via the following commands in PowerShell

    ```sh
    code --install-extension ms-vscode-remote.remote-wsl
    code --install-extension ms-vscode.cpptools
    code --install-extension MS-vsliveshare.vsliveshare-pack
    code --install-extension 13xforever.language-x86-64-assembly
    ```

    Optionally, if you are coming from vim, emacs, or some other editor, and have a working muscle memory for those keybindings, [consider installing a keymap extension](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Relevance).

8. By default, these extensions were installed on your host (Windows) operating system. However, some of these need to run on your Linux guest. To address this, open the VSCode Extensions panel (Ctrl+Shift+X), and look under the heading `LOCAL - INSTALLED`. You will see several extensions that are grayed out and have a button `Install in WSL:Ubuntu:18.04`. Click this for each.
9. You should now see these extensions under `WSL:UBUNTU-18.04 - INSTALLED`. However, they likely have a button `Reload Required`. Once all extensions are installed under Ubuntu, click that button. You may be prompted to enter your Linux password to install additional backend Linux dependencies
10. Hit ctrl+\` to open the integrated shell in VSCode. Visually check that this is your guest (username ubuntu) and not your host environment. We're now ready to install xv6 and its dependencies.

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

11. Assuming everything completed properly, you should boot into xv6, an operating system modeled after version 6 of Bell Labs Research Unix.
12. In xv6, run `cat README`
13. Exit xv6 by hitting ctrl+a (then releasing both keys) and then x.

## Appendix: Troubleshooting

_Note: If you hit a snag, first try restarting your machine before proceeding._

All of the commands in this section assume a PowerShell session started as Administrator.

### _Are you running a new enough version of Windows?_

```powershell
Get-ComputerInfo OsVersion
```

You should see build 19041 or higher

```powershell
OsVersion
---------
10.0.19041
```

### _Have you enabled the Virtualization Extensions of your CPU?_

```powershell
(GWMI Win32_Processor).VirtualizationFirmwareEnabled
```

You should see

```
True
```

If this is false, you need to enter your system BIOS to turn this on.

### _Have you enabled the Virtual Machine Platform feature in Windows 10?_

```powershell
dism.exe /online /Get-FeatureInfo /featurename:VirtualMachinePlatform
```

Among other things, you should see:

```powershell
State : Enabled
```

If this is not enabled, run the following and then restart:

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### _Have you enabled the WSL Subsystem?_

```powershell
dism.exe /online /Get-FeatureInfo /featurename:Microsoft-Windows-Subsystem-Linux
```

Among other things, you should see:

```powershell
State : Enabled
```

If this is not enabled, run the following and then restart:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### _Are you able to query a WSL guest?_

Assuming WSL is setup, you should be able to list a guest and see that it is version 2. The presence of an asterisk indicates the default distribution

```powershell
wsl --list --verbose
```

```
NAME                   STATE           VERSION
* Ubuntu-18.04           Running         2
```

### _Are you able to enter WSL?_

```powershell
wsl
```

This should allow you to enter your default WSL distribution.

If this fails, try the following:

1. Disable WSL

    ```powershell
    dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

1. Restart your system

1. Enable WSL

    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

1. Restart the system

1. Try `wsl` again
