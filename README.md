installing-rtx-5090-on-ubuntu-24.04.2-LTS

Hopefully it will just work with the default nvidia-driver soon

Commands for installing an rtx 5090 on Ubuntu 24.04.2 LTS as of May 1 2025

I only did this one time hoping I got everything right. There maybe a much easier way to do this. I followed a youtube video 


all info from a youtube video by Tech-Practice, couple of small things didn't work for me. I made this to retrace my steps

link: https://www.youtube.com/watch?v=o5deOXLDpZw&t=391s

Date: May 1

fresh install of ubuntu-24.04.2 LTS Server with ssh server installed to ssh into the machine

#update the system

sudo apt update

sudo apt upgrade

sudo apt reboot

#install mainline kernel

sudo add-apt-repository ppa:cappelikan

sudo apt update && sudo apt full-upgrade

sudo apt install -y mainline

mainline list  # list available kernels 

sudo apt install pkexec

sudo mainline install 6.14.4 # choose the latest version

sudo apt install linux-headers-$(uname -r) #make sure its all here

sudo reboot 

#confirm kernel install 

uname -a

Linux 5090 6.14.4-061404-generic #202504251003 SMP PREEMPT_DYNAMIC Fri Apr 25 12:15:31 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux


#prep for compile

sudo apt install build-essential

sudo apt install gcc-14

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-14 14

sudo update-alternatives --config gcc #select the gcc 14

#confirm
gcc --version

sudo reboot # tried to compile without a reboot and it failed, rebooted and it worked 

#download the nvidia driver

#this is the version  (NVIDIA-Linux-x86_64-570.144.run)I downloaded at the time and with sftp I uploaded to the server

chmod +x NVIDIA-Linux-x86_64-570.144.run 

sudo ./NVIDIA-Linux-x86_64-570.144.run #choose mit driver

sudo reboot

#check driver

nvidia-smi    

| NVIDIA-SMI 570.144                Driver Version: 570.144        CUDA Version: 12.8     |


#now lets install ComfyUI

sudo apt install python3.12-venv

git clone https://github.com/comfyanonymous/ComfyUI.git

cd ComfyUI/

python3 -m venv venv

source venv/bin/activate

pip install -U pip setuptools wheel

pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128

pip install -r requirements.txt

cd custom_nodes/

git clone https://github.com/ltdrdata/ComfyUI-Manager.git

cd ..

python main.py --listen #start up comfyui


