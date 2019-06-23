# Installation of dotNET Core to Turris Omnia LXC Container
> This repo is a step-by-step tutorial how to install .NET Core SDK to Turris Omnia LXC container. It's aiming to Windows/PowerShell orientied users which not familiar with SSH/Bash enviroment and the official tutorials are driving them crazy.

## Overview

The installation consists of these steps:

1. Create Debian LXC container
2. Install prerequisites 
3. Deployment of .NET Core libraries to LXC container
4. Hello world!

## 1. Create Debian LXC container

Connect to your Turrins Omnia router by SSH and create the LXC container for .NET Core. **[Official Manual](https://www.turris.cz/doc/en/howto/lxc)**
```
lxc-create -t download -n dotNETCore
```

- Distribution: **Debian**
- Release: **Stretch**
- Architecture: **armv7l** (that is seven and "L" letter, not 1 (digit)

Alternatively, you can use LuCI to create the LXC container. A distribution doesn't not really matter. This tutorial was tested on Debian but it would work more likely on other distribution.

## 2. Simple Debian configuration

Start the container from LuCI or by executing this command:

```
lxc-start -n dotNETCore
```

Connect to container:

```
lxc-attach -n dotNETCore
```

Install prerequisites:

```
apt-get update
apt-get install wget nano git-core openssh-server rsync sudo fakeroot cifs-utils -y
```

SSH is not really necessary but it may become handy sooner or later.

Change hostname (for eg. `dotNETCore`):

```
nano /etc/hostname
```

Set new hostname to localhost:

```
nano /etc/hosts
```

Add this line (for `dotNETCore` as hostname):

```
127.0.1.1   dotNETCore
```

Reboot the LXC container from LuCI interface or execute this command:

```
exit
```
to exit the LXC container SSH session (*which takes you to your Turris Omnia session*) and

```
lxc-stop -n dotNETCore -r
```

to restart the container.

Go to **Foris**, the **DNS** setting tab and turn on the "Domain of DHCP clients in DNS" if you don't have it enabled already. Now, you can access the LXC container by its domain name - **dotNETCore**. 

After restart, you can connect to the container by executing:

```
lxc-attach -n dotNETCore
```

and if your setup was successful, you should see: **root@dotNETCore:~#** in your terminal.

## 3. Deployment of .NET Core libraries to LXC container

Now, let's create a directory for .NET Core SDK and step into it.

```
mkdir -p $HOME/dotnet && cd $HOME/dotnet
```

Now, you can download the .NET Core SDK binaries:

```
wget https://download.visualstudio.microsoft.com/download/pr/50bc5936-b374-490b-9312-f3ca23c0bcfa/d7680c7a396b115d95ac835334777d02/dotnet-sdk-3.0.100-preview6-012264-linux-arm.tar.gz
```

You can customize this link to whatever version of .NET Core SDK you want.

After download, you have to unpack the binaries:

```
tar zxf ./dotnet-sdk-3.0.100-preview6-012264-linux-arm.tar.gz -C $HOME/dotnet
```

For an easy use of .NET environment, you can execute:

```
nano ~/.bashrc
```

and enter these lines at the end of the file:
```
export PATH=$PATH:$HOME/dotnet
export DOTNET_ROOT=$HOME/dotnet
```

This will create dotnet environmental variables everytime when you log in to the container by SSH.

Now, you can execute

```
exit
```
to exit the LXC container SSH session (*which takes you to your Turris Omnia session*) and

```
lxc-stop -n dotNETCore -r
```

to restart the container. After restart, execute:

```
lxc-attach -n dotNETCore
```

to attach to the container again. Now, you should have all set and good to go.

## 4. Hello world!

Try to run following command:

```
dotnet
```

if you see following outcome (*it may change overtime*)

```
Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET Core information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes.

path-to-application:
  The path to an application .dll file to execute.
```

go ahead and try the **Hello world** console app.

```
dotnet new console -o myApp && cd myApp
dotnet run
```

If you see:

```
Hello World!
```

congratulation! You have fully operational .NET Core SDK up and running in the LXC container hosted in your Turris Omnia.

If you receive the error "bash: dotnet: command not found" navigate manually to it:

```
cd $HOME/dotnet
```

and execute the `dotnet` command here. Alternatively, you can use the full path classification:

```
$HOME/dotnet/dotnet
```

## Links
[Download .NET Core 3.0 Portal](https://dotnet.microsoft.com/download/dotnet-core/3.0)<br>
[Build your first .NET Core app](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro)<br>
[dotNET Core 3.0 SDK - Linux ARM32 Binaries (v3.0.100-preview6)](https://dotnet.microsoft.com/download/thank-you/dotnet-sdk-3.0.100-preview6-linux-arm32-binaries)<br>

## Credits

All goes to .NET Core team. YOU ROCK!
  
