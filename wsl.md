# Windows Subsystem for Linux

[Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) (WSL) provides a fully functional Linux shell. I use it when working with bash scripts. 

- WSL provides a full Linux kernel and environment.
- It supports bash scripting and standard Linux tools without path translation issues.
- It can seamlessly interact with Windows files and directories (via /mnt).


## WSL Clean Slate

```bash
# Check for existing WSL servers
wsl --list --verbose

>>   NAME                   STATE           VERSION
>> * docker-desktop         Running         2
>>   docker-desktop-data    Stopped         2

# Stop running servers
wsl --terminate docker-desktop
wsl --terminate docker-desktop-data
wsl --list --verbose

# Unregister servers
wsl --unregister docker-desktop
wsl --unregister docker-desktop-data

wsl --list --verbose
>> Windows Subsystem for Linux has no installed distributions.

# Exit WSL and kill terminal.

# Delete `C:\Users\wcd\.wslconfig` if it exits - may be hidden.
# I do not have this file.

# In Powershell as admin,
# reset network settings to clear old virtual switches. 
netsh winsock reset

>> Successfully reset the Winsock Catalog.
>> You must restart the computer in order to complete the reset.

# After restarting, reset WSL entirely.
wsl --shutdown
dism.exe /online /cleanup-image /restorehealth

# Restart computer again.

```

## Install WSL

[Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

```bash
# As admin
# Check if WSL is installed.
wsl --version
>> WSL version: 2.3.26.0

# If not, install it...
wsl --install

# ...And ensure it is enabled at
# Start > Turn Windows Features On or Off

# Start shell
wsl
>> Windows Subsystem for Linux has no installed distributions.
>> Use 'wsl.exe --list --online' to list available distributions
>> and 'wsl.exe --install <Distro>' to install.

# View a list of Linux distributions
wsl --list --online

# Instal and Ubuntu
# You will create a UNIX username and password
# Be sure to save this to a password manager. 
wsl --install -d Ubuntu

>> This message is shown once a day. To disable it please create the
>> /home/wcd/.hushlogin file.

sudo apt update && sudo apt upgrade -y

# Test bash script execution
echo -e '#!/bin/bash\n\necho "Hello, WSL!"' > test-script.sh
chmod +x test-script.sh
./test-script.sh
>> Hello, WSL!

# Test bash script execution from WSL shell
cd /mnt/f/scripts
chmod +x hello-world.sh
./hello-world.sh
>> Hello, World!
exit

# Test bash script execution from PowerShell
# as a non-admin
wsl bash /mnt/f/scripts/hello-world.sh
>> Hello, World!
```

# Install HUGO on WSL

I wrote some bash scripts to help set up new [HUGO sites](./hugo/README.md). This required installing HUGO in WSL. 

```bash
# PowerShell as admin
wsl
sudo apt update
sudo apt install hugo
hugo version
```

# Creating Directories in Bash scripts

``` bash
>> [?] Directory path for Hugo site (F:/my-new-site):
>> F:/hugo-wsl-one
>> [✔] Directory created: F:/hugo-wsl-one
>> [✔] Changed to directory: /mnt/c/Users/wcd/F:/hugo-wsl-one

# Directory created at:
"C:\Users\wcd\F\hugo-wsl-one"


[?] Directory path for Hugo site (F:/my-new-site):
F:/hugo-wsl-two
[✔] Directory created: /mnt/f/hugo-wsl-two
[✔] Changed to directory: /mnt/f/hugo-wsl-two





```
