# Tips and Tricks for Linux development with WSL and Visual Studio Code

In an earlier blog post, [An In Depth Tutorial on Linux development on Windows with WSL and Visual Studio Code](https://devblogs.microsoft.com/commandline/an-in-depth-tutorial-on-linux-development-on-windows-with-wsl-and-visual-studio-code), we showed you how to set up [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl) (WSL) and [Visual Studio Code](https://code.visualstudio.com) for Linux development. In this post, we'll go into more detail and provide tips and tricks to further enhance Linux development on Windows.

## Remote - WSL extension

The features described below are provided by the VS Code [Remote – WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). If you don't have the extension already installed, open the Extensions view in VS Code (Ctrl+Shift+X) and search for "wsl".  Choose the **Remote – WSL** extension as seen below (it should be at the top of the list) and press **Install**.

![Remote - WSL extension](remote-wsl-extension.png)

## Workaround for renaming folders

In the current version of WSL (WSL1), there is a limitation where it is not possible to rename a non-empty folder from VS Code. To work around this [issue](https://github.com/microsoft/WSL/issues/3395), you can tell VS Code to "poll" for file system changes rather than apply a lock to the folder.

In your user `settings.json`, add:

```json
"remote.WSL.fileWatcher.polling": true
```

Polling is resource heavy, so it is not turned on by default. You can also tune how often VS Code polls using the `remote.WSL.fileWatcher.pollingInterval` setting, which is by default every 5 seconds.

You will need to reload VS Code (**Developer: Reload Window** from the Command Palette (F1)) for these settings to take effect. The next version of WSL ([WSL2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders), which is in preview) fixes this issue and provide significantly better file system performance.

## Multiple distro support

You can install multiple Linux distros on Windows. If you followed the tutorial in the earlier blog post, you installed the Ubuntu "LTS" (Long Term Stable) release, which is currently version 18.04. You can install another distro such as Debian from the store (just search for "Debian"), such that you end up with two different distros installed.

![Multiple Linux distros installed](multiple-distros.png)

If you are connected to one distro and want to open a new VS Code window, bring up the Command Palette (F1), search for "distro" and choose the command **Remote-WSL: New Window using Distro…**. This will bring up a quick pick that will let you choose between the different distros you have installed. Once you pick a distro, a new instance of VS Code will open connected to the appropriate distro.

![Select WSL distro](select-wsl-distro.png)

**Note**: WSL from Windows 10, May 2019 Update (version 1903) is required for this feature.

You can also run the **Remote-WSL: New Window using Distro…** command from the Remote - WSL command dropdown displayed when you click on the remote development Status bar item on the far left.

![Remote - WSL commands](remote-wsl-commands.png)

## Switching between Windows (local) and WSL (remote) workspaces

Sometimes you need to switch between local workspaces (folders) in Windows and remote workspaces in WSL.

Here are a few commands that will make switching a snap:

* When you are connected to a WSL instance, the **File** > **Open Folder** command will show the Linux file system along with a **Show Local** command to open the Windows File System dialog.
* The **Remote–WSL: Reopen Folder in Windows** (or **in WSL**) commands will reopen the same folder either in Window or in WSL, depending on how you are currently connected. If you are connected to a WSL instance, then the command will open the folder under the `\\wsl$` mount on Windows. Conversely, if you are on Windows, then it will open the folder natively on Linux.
* If you are connected to a remote WSL instance, you can click on the **WSL** Status bar item and then choose **Close Remote Connection**, which will bring you back to an empty VS Code window (instance) where you can open a local Windows folder as before.

## Setting the default shell

You can easily set up different default shells when opening a terminal in VS Code. For example, when running on the Windows side, you can specify PowerShell or WSL. When running in WSL, you can choose bash or zsh or whatever shell you might have installed.

Open a new terminal **Terminal** > **New Terminal** (Ctrl+`) and open on the dropdown. Choose **Select Default Shell** and if you are on the Windows side, you'll see Command Prompt, PowerShell, or WSL Bash:

![Select default shell on Windows](select-default-shell-windows.png)

If you are connected to a WSL instance, you'll see all of the shells defined in /etc/shells:

![Select default shell on WSL](select-default-shell-wsl.png)

## Alpine distro support

It's still in the [experimental stage](https://github.com/microsoft/vscode-docs/blob/master/remote-release-notes/v1_37.md#wsl) and requires you use the VS Code [Insiders](https://code.visualstudio.com/insiders) build, but you can run VS Code in Alpine distributions.

![Alpine distribution with VS Code](alpine-distro.png)

## Linux development on Windows

WSL and VS Code let you do productive Linux development from the convenience of your Windows machine. To learn more, see the VS Code [Remote Development documentation](https://code.visualstudio.com/docs/remote/remote-overview), where you'll find [guides](https://code.visualstudio.com/docs/remote/wsl) and [tutorials](https://code.visualstudio.com/remote-tutorials/wsl/getting-started).

And keep following the [Windows Command Line Tools blog](https://devblogs.microsoft.com/commandline/) and the [Visual Studio Code release notes](https://code.visualstudio.com/updates) for further improvements to WSL and the Remote - WSL extension.

Happy Coding
