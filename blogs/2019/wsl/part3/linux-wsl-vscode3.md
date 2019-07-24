# Linux development with WSL and Visual Studio Code Part 3

<!-- TBD intro and link to part 2 -->

## Multiple distro support

You can install multiple Linux distros on Windows. In this [Linux development with WSL and VS Code Part 2](TBD), we first installed the Ubuntu "LTS" (Long Term Stable) release, which is currently version 18.04. You can install another distro such as Debian from the store (just search for "Debian"), such that you end up with two different distros installed.

![multiple Linux distros](multiple-distros.png)

If you are connected to one distro and want to open a new VS Code window, bring up the Command Palette (F1), search for "distro" and choose the command **Remote-WSL: New Window using Distro…**. This will bring up a quick pick that will let you choose between the different distros you have installed, and once you pick on a new instance of VS Code will open connected to the appropriate distro.

![select WSL distro](select-wsl-distro.png)

> Note: WSL from Windows 10, May 2019 Update (version 1903) is required for this feature.

## Switching between Windows (local) and WSL (remote) workspaces

Sometimes you need to switch between local workspaces (folders) in Windows and remote workspaces in WSL. Here are a few commands that will make this a snap.

* When you are connected to a WSL instance, the **File** > **Open Folder** command will show the Linux file system along with a **Show Local** command to open the Windows File System dialog.
* The **Remote–WSL: Reopen Folder in Windows** (or **in WSL**) commands will reopen the same folder either in Window or in WSL, depending on how you are currently connected. If you are connected to a WSL instance, then the command will open the folder under the `\\wsl$` mount on Windows. Conversely, if you are on Windows, then it will open the folder natively on Linux.
* If you are connected to a remote WSL instance, you can click on the **WSL** Status bar item and then choose **Close Remote Connection**, which will bring you back to an empty VS Code window (instance) where you can open a local Windows folder as before.

## Setting the default shell

You can easily set up different default shells when opening a terminal in VS Code. For example, when running on the Windows side, you can specify PowerShell or WSL. When running in WSL, you can choose bash or zsh or whatever shell you might have installed.

Open a new terminal **Terminal** > **New Terminal** (Ctrl+`) and open on the dropdown. Choose **Select Default Shell** and if you are on the Windows side, you'll see Command Prompt, PowerShell, or WSL Bash:

![select default shell on Windows](select-default-shell-windows.png)

If you are connected to a WSL instance, you'll see all of the shells defined in /etc/shells:

![select default shell on WSL](select-default-shell-wsl.png)

## Renaming folders

In this version of WSL (WSL1), there is a limitation where it is not possible to rename a non-empty folder from VS Code. To work around this issue, you can tell VS Code to "poll" for file system changes rather than apply a lock to the folder.

In settings.json, add:

```json
"remote.WSL.fileWatcher.polling": true
```

Polling is resource heavy, so it is not turned on by default. You can also tune of often VS Code polls using the "remote.WSL.fileWatcher.pollingInterval" setting, which is by default every 5 seconds.

You will need to reload VS Code (**Reload Window** from the Command Palette (F1)) for these settings to take effect. The next version of WSL ([WSL2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders), which is in preview) fixes this issue and provide significantly better file system performance.

<!-- TBD wrap up and next steps -->

Happy Coding
