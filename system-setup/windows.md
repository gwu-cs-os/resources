# Setup for Windows 10 Users

The suggested toolkit for Windows 10 users is Windows Subsystem for Linux versions 2 (WSL 2).

This environment runs a headless Ubuntu virtual machine in a lightweight virtual machine, with various interop features with the host Windows 10 environment.

## Minimum Requirement

-   Windows 10, version 2004, Build 19041 or higher. Check this by typing `Windows Logo Key` + R, typing `winver` in the Run window, and selecting OK.

## Installation Instructions

1. Install the new Windows Terminal. This generally does a better job with Linux shells, and it makes it easier to toggle between PowerShell, DOS, and (once installed) your Ubuntu 20 guest. https://www.microsoft.com/store/productId/9N0DX20HK701
2. Install the WSL 2 environment. https://docs.microsoft.com/en-us/windows/wsl/install-win10. When you get to the _Install your Linux distribution of choice_ step, select Ubuntu 20 LTS

    Note: If you see `Error: 0x1bc` or a message similar to `WSL 2 requires an update to its kernel component. For information please visit https://aka.ms/wsl2kernel` when you run `wsl --set-default-version 2`, download and install the kernel update from https://docs.microsoft.com/nl-nl/windows/wsl/wsl2-kernel

3. Verify that your Linux guest is using version 2 by running `wsl --list --verbose` in a PowerShell session
4. Open a terminal (using `ssh` or using VSCode by hitting ctrl+\` to open the integrated shell). Visually check that this is your guest (username ubuntu) and not your host environment. We're now ready to install xv6 and its dependencies.

    ```sh
    sudo apt-get update --yes
    sudo apt-get install build-essential gdb glibc-doc qemu-system-x86 --yes
    cd ~
    mkdir projects
    cd projects
    git clone https://github.com/gwu-cs-os/gwu-xv6
    cd gwu-xv6
    make qemu-nox
    ```

5. Assuming everything completed properly, you should boot into xv6, an operating system modeled after version 6 of Bell Labs Research Unix.
6. In xv6, run `cat README`
7. Exit xv6 by hitting ctrl+a (then releasing both keys) and then x.

## Appendix: VSCode (**optional**)

1. Install VSCode and complete this tutorial https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode up to _Extensions inside of VS Code Remote_
1. Close VSCode and open a Linux shell in Windows Terminal. Type `code .` in your home directory and ensure that you are able to launch VSCode with an open Linux directory.
1. Hit ctrl+` in VSCode, and ensure that the terminal in VSCode opens to your Linux home directory.
1. Install the following extensions:

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
1. By default, these extensions were installed on your host (Windows) operating system. However, some of these need to run on your Linux guest. To address this, open the VSCode Extensions panel (Ctrl+Shift+X), and look under the heading `LOCAL - INSTALLED`. You will see several extensions that are grayed out and have a button `Install in WSL:Ubuntu:20.04`. Click this for each.
1. You should now see these extensions under `WSL:UBUNTU-20.04 - INSTALLED`. However, they likely have a button `Reload Required`. Once all extensions are installed under Ubuntu, click that button. You may be prompted to enter your Linux password to install additional backend Linux dependencies

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
* Ubuntu-20.04           Running         2
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

### _Are you unable to run apt due to clock drift?_

A student reported that `sudo apt update` failed with the following:
```
Hit:1 https://packages.microsoft.com/repos/azure-cli bionic InRelease
Hit:2 http://archive.ubuntu.com/ubuntu bionic InRelease
Get:3 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Hit:5 http://archive.ubuntu.com/ubuntu bionic-backports InRelease
Get:4 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Reading package lists... Done
E: Release file for http://security.ubuntu.com/ubuntu/dists/bionic-security/InRelease is not valid yet (invalid for another 31min 12s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/bionic-updates/InRelease is not valid yet (invalid for another 11h 0min 22s). Updates for this repository will not be applied.
 ```

Run the following script to detect and correct clock drift.

```bash
#!/bin/bash
# Based on the following error conditions
# 1: https://github.com/microsoft/WSL/issues/4114
# 2: https://askubuntu.com/questions/1096930/sudo-apt-update-error-release-file-is-not-yet-valid/1168885#1168885

main() {
  local -i unix_hours
  local -i unix_minutes
  local -i unix_seconds
  local -i win_hours
  local -i win_minutes
  local -i win_seconds
  local -i total_drift_secs

  win_seconds=$(powershell.exe /C Get-Date -Format ss | sed 's/^0*//' | tr -d '\r')
  unix_seconds=$(date '+%S' | sed 's/^0*//')
  win_minutes=$(powershell.exe /C Get-Date -Format mm | sed 's/^0*//' | tr -d '\r')
  unix_minutes=$(date '+%M' | sed 's/^0*//')
  win_hours=$(powershell.exe /C Get-Date -Format HH | sed 's/^0*//' | tr -d '\r')
  unix_hours=$(date '+%H' | sed 's/^0*//')

  printf "Unix: %d:%d:%d\n" "$unix_hours" "$unix_minutes" "$unix_seconds"
  printf "Windows: %d:%d:%d\n" "$win_hours" "$win_minutes" "$win_seconds"

  total_drift_secs=$(((win_hours - unix_hours) * 60 * 60 + (win_minutes - unix_minutes) * 60 + (win_seconds - unix_seconds)))
  if ((total_drift_secs < 0)); then
    ((total_drift_secs *= -1))
  fi

  printf "Drift: %ds\n" "$total_drift_secs"
  if ((total_drift_secs > 60)); then
    echo "Correcting Drift"
    sudo hwclock --hctosys
  fi
}

main "$@"

```
