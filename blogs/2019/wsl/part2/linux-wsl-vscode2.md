# Linux development with WSL and Visual Studio Code Part 2

In an earlier blog post, [Take your Linux development experience in Windows to the next level with WSL and Visual Studio Code Remote](https://devblogs.microsoft.com/commandline/take-your-linux-development-experience-in-windows-to-the-next-level-with-wsl-and-visual-studio-code-remote/), we introduced the VS Code [Remote - WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl), which simplifies Linux development on Windows Subsystem on Linux (WSL). In Part 2, we'll walk you through setting up WSL and VS Code for Python development and create a Python "Hello World" application.

## Windows: A great platform for building Linux Apps

Windows is the most popular operating system in the world, with [almost 50% of developers](https://insights.stackoverflow.com/survey/2019#technology-_-developers-primary-operating-systems) using it every day. At the same time, many of these developers are building applications that are [deployed to Linux-based](https://insights.stackoverflow.com/survey/2019#technology-_-platforms) servers running in the cloud or on-premises.

With the WSL and VS Code, you can now seamlessly develop Linux-based applications on Windows. WSL lets you run a full Linux distro on Windows, where you can install platform-specific toolchains, utilities, and runtimes.

VS Code and the WSL extension let you develop in the context of the Linux environment, using those tools and runtimes, from the comfort of Windows. All of your VS Code settings are maintained across Windows and Linux, making it easy to switch back and forth. Commands and workspace extensions are run directly in Linux, so you don't have to worry about pathing issues, binary compatibility, or other cross-OS challenges. You're able to use VS Code in WSL just as you would from Windows. One tool, two operating systems.

If it sounds magical, it is :-). But, don't take our word for it. Let's get our hands dirty and build a simple Python3 application so you can experience the magic for yourself.

## Install and set up WSL

You install WSL from the Windows Store. You can use the store app, or simply search for "Ubuntu" in the Windows' search bar. Choose the Linux distribution you want to install and follow the prompts.

![select Ubuntu distro](select-distro.png)

Select **Install**.

![install Ubuntu](install-ubuntu.png)

And when done, select **Launch** to get started. This will open a Linux terminal and complete the installation.  You'll need to create a user ID and password since we're setting up a full Linux instance, but once that's done, boom! You are running Linux on Windows.

![Linux terminal](linux-terminal.png)

## Python development

If you don't have Python already installed, run the following commands to install Python3 and pip, the package manager for Python, into your Linux installation.

```bash
sudo apt update
sudo apt install python3 python3-pip
```

And to verify, run:

```bash
python3 –version
```

This isn't intended to be a Python tutorial, so we'll do the canonical "Hello World" app. Create a new folder called "helloWorld" and then add a Python file that will print a message when run:

```bash
mkdir helloWorld && cd helloWorld
echo 'print("hello from python on ubuntu on windows!")' >> hello.py
python3 hello.py
```

Clearly, echo isn't a great way to do development. In a remote Linux environment (this WSL distro is technically another machine without UI, that just happens to be running locally on your computer), your development tools and experiences are pretty limited.  You can run [Vim](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/) in the terminal to edit your file or you can edit the sources on the Windows side through the `\\wsl$` mount:

![\\wsl$ mount](wsl$-mount.png)

The problem with this model is that the Python runtime, pip, or any conda packages for that matter, are not installed on Windows.

![no Python on Windows](no-python-on-windows.png)

Remember, Python is installed in the Linux distro, which means if we're editing Python files on the Windows side, we can't run or debug them unless we install the same Python development stack on Windows. And that defeats the purpose of having an isolated Linux instance set up with all our Python tools and runtimes!

What can we do?  Visual Studio Code and the Remote – WSL extension to the rescue.

## Visual Studio Code

VS Code is a lightweight, cross platform source code editor, built on open source. It comes with built-in support for modern web development with JavaScript, TypeScript, Node.js, CSS, etc. It also has a rich ecosystem of extensions (10K+) providing support for 100s of languages and frameworks, such as Python, Go, PHP, Java, C++, and C#.

If you don't already have VS Code, [download it now](https://code.visualstudio.com). It's about 50 MB to download on Windows and sets up in less than a minute.

Now we just need the magic, and that is the [Remote – WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Open the Extensions view in VS Code (Ctrl+Shift+X) and search for "wsl".  Choose the **Remote – WSL** extension as seen below (it should be at the top of the list) and press **Install**.

![Remote - WSL extension](remote-wsl-extension.png)

Once installed, head back over the WSL terminal, make sure you are in the helloWorld folder, and type in "code ." to launch VS Code (the "." tells VS Code to open the current folder).

![launch VS Code](launch-code.png)

The first thing you'll see is a message about "Installing VS Code Server" (the c7d83e57… number is the version of the VS Code Server that matches the client-side tools you just installed). VS Code is installing a small server on the Linux side that the desktop VS Code will then talk to.

![vscode server](vscode-server.png)

That server will then install and host extensions in WSL, so that they run in the context of the tools and frameworks installed in WSL. In other words, your Python extension will run against the Python installed in WSL, not against what is installed on the Windows side, as it should for the proper development experience.

The next thing that happens is VS Code will start and open the helloWorld folder. You may see a quick notification telling you that VS Code is connecting to WSL, and you may be prompted to allow access to the Node.js-based server.

![installing vscode server](installing-vscode-server.png)

Now, when we hover over hello.py, we get the proper Linux path.

![show hello.py Linux path](show-linux-path.png)

## Integrated Terminal

If that doesn't convince you we're connected to the Linux subsystem, run **Terminal** > **New Terminal** (Ctrl+`) to open a new terminal instance.

![new terminal in WSL](new-terminal-in-wsl.png)

You'll start a new instance of the bash shell in WSL, again from VS Code running on Windows.

**Tip**: In the lower left corner of the Status Bar, you can see that we're connected to our **WSL: Ubuntu** instance.

![Remote - WSL Status bar](wsl-status-bar.png)

Click on it to bring up a set of Remote - WSL extension commands.

![Remote - WSL commands](remote-wsl-commands.png)

## Installing the Python extension (and additional tools)

Click on hello.py to open it for editing. We are prompted with what we call an "Important" extension recommendation, in this case to install the Python extension, which will give us rich editing and debugging experiences. Go ahead and select **Install** and reload if prompted.

![Python extension recommendation](python-extension-recommendation.png)

To prove that the extension is installed in WSL, open the Extensions view again (Ctrl+Shift+X). You will see a section titled **WSL – Installed** and you can see any extensions that are installed on the WSL side.

![WSL installed extensions](wsl-installed-extensions.png)

Upon reload, you'll also get prompted telling you that the pylint linter is not installed. Linters are used to give us errors and warnings in our source code. Go ahead and select **Install**.

![pylint not installed notification](pylint-not-installed.png)

Now, when you edit your code, you get rich colorization and completions.

![Python IntelliSense](python-intellisense.png)

And when you save your file (Ctrl+S), you'll get linting errors and warnings on the file.

![pylint error](pylint-error.png)

## Debugging

With our tools set up, let's take this one step further. Set a breakpoint on line 1 of hello.py by clicking in the gutter to the left of the line number or by putting the cursor on the line and pressing F9.

![set breakpoint](set-breakpoint.png)

Now, press F5 to run your application. You will be asked how to run the application, and since this is a simple file, just choose **Python File**.

![select debug configuration](select-debug-config.png)

The app will start, and you'll hit the breakpoint. You can inspect variables, create watches, and navigate the call stack.

Press F10 to step and you'll see the output of the print statement in the debug console.

![VS Code debug view](debug-view.png)

You get the full development experience of Visual Studio Code, using the Linux instance installed in WSL.

If you want to open another folder in WSL, open the **File** menu and choose **Open Folder**. You'll get a minimal file and folder navigator for the Linux file system, not the Windows file system.

![open folder navigator](open-folder.png)

If you want to switch back to the Windows, select the **Show Local** option and you'll get the standard Windows File Open dialog.

## In conclusion

![Doug Henning magic](magic.png)

## More information

With WSL, VS Code and the Remote – WSL extension, your Windows machine becomes an awesome box for developing Linux applications. Your tools run on Windows while your application runs where it will be deployed, on Linux.

Linux development is not limited to WSL. Using the [Remote – SSH extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh), you can develop against remote SSH hosts with the same fidelity as shown here, all from your Windows desktop.

For more information, check out the following resources:

* [Remote development overview](https://code.visualstudio.com/docs/remote/remote-overview)
* [Remote WSL documentation](https://code.visualstudio.com/docs/remote/wsl)
* [Remote development with Docker Containers](https://code.visualstudio.com/docs/remote/containers)
* [Remote development over SSH](https://code.visualstudio.com/docs/remote/ssh)
* [WSL Microsoft Learning module](https://docs.microsoft.com/en-us/learn/modules/get-started-with-windows-subsystem-for-linux)
* [Windows Subsystem for Linux documentation](https://docs.microsoft.com/windows/wsl/install-win10)
* [Get started with Python in Visual Studio Code](https://code.visualstudio.com/docs/python)

Finally, if you really want to supercharge your Windows dev box, try out the new [Windows Terminal](https://devblogs.microsoft.com/commandline/introducing-windows-terminal)!

## Stay tuned

Look for "Linux development with WSL and VS Code" Part 3 coming soon, which will have development tips and tricks on working with multiple Linux distros, switching between Linux and Windows folders, and changing the default shell.

HappyCoding
