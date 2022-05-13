# Installation Guide for <ins>W</ins>indows <ins>S</ins>ubsystem for <ins>L</ins>inux <ins>2</ins>

## Table of content

- [Check your Windows version](#windows)
- [Install/Update the NVIDIA driver](#driver)
- [Install WSL2](#wsl)
- [Install softwares](#software)
- [Trouble shooting](#shoot)
- [Additional Links](#link)

  
## Check your Windows version <a id="windows">
  
Windows 11 or Windows 10, version 21H2 is needed to enable GPU acceleration. If you are using Windows 10 version 21H2, you might need to be in the **dev channel**. I personally tested with Windows 11 and can confirm that it is not need for Windows 11. If you don't have access to any required system, you could try installing WSL2 with CPU only instead.
  
To check your Windows version: 
  
- Windows 11: Windows -> Settings -> System -> About
- Windows 10: Windows -> Settings -> About
  
To upgrade your system, there are a few options:
  
- Windows -> Settings -> Search for Windows Update -> Look for upgrade options.
- Directly download and install Windows 11/10 by [Windows 11 installer](https://www.microsoft.com/software-download/windows11)/[Windows 10 installer](https://www.microsoft.com/en-us/software-download/windows10).
- To try out Dev channel, please consult this website [Windows insider program](https://docs.microsoft.com/en-us/windows-insider/get-started).
  
## Install/Update the NVIDIA driver <a id="driver">
  
Please [download and install the appropriate NVIDIA driver](https://www.nvidia.com/download/index.aspx) on your **Windows** system. In earlier times, there's a seperate driver for WSL but recently NVIDIA merged it into the currently available drivers. I personally installed the Game Ready Driver(GRD), but I suppose the Studio Drivers also have it. 
  
Note that we will **not** need to install any drivers in the Linux system.
  
## Install WSL2 <a id="wsl">
  
If WSL2 is not installed at all, please the following command in your Windows terminal/powershell. This command will download and set up WSL2 with Ubuntu by default
```
wsl --install
```
If you want some other Linux distribution, you can run the following 
```
wsl.exe --install -d <Distribution Name>
```
After the above is done, please go to Windows and search for the distribution that you installed. Take Ubuntu for example, we search for "Ubuntu for Windows". Open it and you should see a line of words saying
  

> Installing, this may take a few minutes ...
  
Giving some time to install, it should be ready to go in a few minutes. However, you might see something like
  
> The virtual machine could not be started because a required feature is not installed
  
In this case, there are a few things can be done:
- Enable Virtual Machine Platform, Windows Subsystem for Linux, Windows Hypervisor Platform: <br>Windows -> Search for Turn Windows features on or off -> Look for these features
- Enable Hardware/CPU virtualization:<br>Restart the machine -> go into BIOS mode -> enable virtualization (may have different names depending on you device)
  
We have successfully installed WSL2 with GPU acceleration by now. To verify that we do have GPU access, we can simply install Pytorch with CUDA and run the following in the python terminal (guide available in the next section)
```
import torch
torch.cuda.is_available()
torch.cuda.get_device_name(0)
```
  
  
## Install softwares <a id="software">
  
Now you can install your favorite softwares as how you would usually do! 
  
- Miniconda
  
Run the following and restart the command shell
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```
  
- Pytorch
  
Go to [Pytorch's official website](https://pytorch.org/get-started/locally/) and choose the desired version. For example, to install pytorch 1.11.0 with CUDA 11.3
```
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```

- CUDA
  


  
## Trouble shooting <a id="shoot">
- WSL2 cannot connect to internet: This is usually because the DNS server address is constantly rewritten by WSL2 for some reason. Simply run the following command fixes the problem
```
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "[network]" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
sudo chattr +i /etc/resolv.conf
```
  
## Additional Links <a id="link">
- WSL2 Official guide: https://docs.microsoft.com/en-us/windows/wsl/install 
- WSL2 Official trouble shooting: https://docs.microsoft.com/en-us/windows/wsl/troubleshooting#installation-issues 
- NVIDIA CUDA on WSL User Guide: https://docs.nvidia.com/cuda/wsl-user-guide/index.html#getting-started-with-cuda-on-wsl
- How do I make Ubuntu 20.04 work on Windows 10 WSL 2: https://askubuntu.com/questions/1275748/how-do-i-make-ubuntu-20-04-work-on-windows-10-wsl-2
