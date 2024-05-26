---
layout: post
title:  How do I setup Isaac Gym Preview Release on Ubuntu?
description: >
  In this blog, I discuss the steps to setup an AWS instance for running IsaacGym on a cloud GPU 
image: /assets/img/blog/aws_ig/cloud-gpu.png
sitemap: false
---

# Working with Isaac Gym using an AWS instance

Isaac Gym is a software that requires a high-level GPU for running its computationally and graphically expensive simulations. But using such high-level GPU hardware is not an option everyone has, given the cost of purchasing one might be out of the comfort zone for a beginner. Hence, using a cloud GPU service such as an AWS EC2 instance with the required specifications can be helpful in running Isaac Gym for an enthusiast or a student who prefers to try out the simulator before investing on the hardware or opts for a cloud GPU rather than a local setup.

## Creating an AWS EC2 Instance

To create an AWS instance for working on Isaac Gym remotely, just take care in selecting an instance with the required options. I use the AWS instance tailored to run Isaac Sim specifically since I initially intended to use Isaac Sim. This instance is good enough to run Isaac Gym but you can choose any better alternative as well. If you intend to use this same instance/ want to use any other but are new to using AWS, you can use the following link to an instruction video by NVIDIA for setting up the AWS instance: 

[https://www.youtube.com/live/CG7qzSiw9Eo?si=-H-69aEWLdRuV7NL](https://www.youtube.com/live/CG7qzSiw9Eo?si=-H-69aEWLdRuV7NL)

Some of the important points discussed about creating a key file, deciding the Security Group rules, the Number of vCPUs, the Volume size (GiB) can be adopted directly from the instructions given in the video. 

## Working on the AWS EC2 Instance

### Starting up the AWS Instance

The steps to start and connect your AWS instance are pretty simple.

1. Start the EC2 Instance by right-clicking on the name of the instance and clicking on *Start Instance* on the drop down menu.
2. Connect to the Instance by clicking on *Connect*, again from the drop down menu.
3. In the new page opened *‘Connect to Instance’*, select the *‘SSH’* tab and copy the command at the end of the page.
4. Replace *‘root’* with *‘ubuntu’* before executing this command on the terminal on your local Ubuntu machine. 

This should connect you to the instance.

### Stopping the AWS EC2 Instance

To stop a running instance, simply go back to the menu of available EC2 instances, and select the concerned instance. Right-click on it and click on *Stop Instance* from the drop-down.

### Installing Isaac Gym on the Remote Instance

Once the instance is connected on your local machine, you can install the Isaac Gym Preview Release on your instance. My blog [**How do I setup Isaac Gym Preview Release on Ubuntu?**](https://amoghjuloori.github.io/blogs/2023-08-10-How-do-I-setup-Isaac-Gym-Preview-Release-on-Ubuntu/) can be helpful in guiding you to do this. 

Just check your NVIDIA driver version and CUDA version on your remote instance. I use the following versions:

1. NVIDIA driver version: 525
2. CUDA version: 10.6

You can check the same using `nvidia-smi` **for the driver version and `nvcc --version` for the CUDA version. You can check the compatibility of your driver and CUDA versions at:  

[1. Why CUDA Compatibility — CUDA Compatibility r550 documentation](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

When trying to change your driver/CUDA version to arrange for compatibility between them, you might encounter some errors, which is common. Such errors can be easily dealt with by searching for solutions on the internet. 

For example, *Failed to initialize NVML: Insufficient Permissions* is an error I encountered when I ran `nvidia-smi` after making changes to my driver version. This can be dealt with by changing permissions again by running:

```bash
sudo chmod 666 /dev/nvidia*
```

### Transfer of files between local machine and remote machine

When working on a remote machine, there might be times when you would like to transfer files from your local machine to the remote machine. Hence, for transferring files from local machine to AWS instance, use:

```bash
scp -i ./key-pair.pem ./path_to_file_on_local_machine/ <username>@<public-ip>:/path_to_copy_on_instance
```

 here `key-pair.pem` **is the key you had created earlier when creating your instance. 

In case you require to transfer an entire folder, we add the suffix *-r* to iterate through the contents of the folder/directory recursively:

```bash
scp -i ./key-pair.pem -r ./path_to_folder_on_local_machine/ <username>@<public-ip>:/path_to_copy_on_instance
```

If want to do the opposite, i.e. transfer from the instance to your local machine, you can use:

```bash
scp -i ./key-pair.pem <username>@<public-ip>:/path_to_file_on_instance/ ./path_to_copy_on_local_machine 
```

Note: All the `scp` commands are run on the **local machine** **only.**

## **Using NoMachine for Display**

I use the NoMachine display tool for my virtual display since it’s a free tool useful for working on the instance when we want to use GUI for opening and running the applications on the remote PC. It provides us with multiple options, such as one which allows for prioritising between Speed and Quality of streaming, change in refresh rate, resolution etc. 

You can explore more about it at their website: [https://www.nomachine.com/](https://www.nomachine.com/)

If you are interested in using NoMachine, you just have to create a connection with your desired name on the NoMachine application. **The connection type is of NX which uses Port 4000, hence you will need to add a new rule to your In-bound rules for your instance’s Security Group on the AWS portal**. In the *Host* section, you will need to enter the public IP of the instance, which changes every time you connect to it, hence you’ll need to change this every time you connect to the instance.Then you can click on Connect and agree to the terms and your display will be ready to use.

 

Each time you connect to the instance, there are a few steps you need to follow to start the NoMachine display successfully: 

1. Edit the *Host IP* in your connection and click Connect. 
2. Refresh the NX server for NoMachine on your instance’s terminal using:

```bash
sudo /etc/NX/nxserver --restart
```

1. Check the Display Settings for NoMachine on the instance’s terminal using:

```bash
#check the DISPLAY being used
echo $DISPLAY

#if the above command returns nothing, assign value 1001 here....
export DISPLAY=:1001
```

1. You can run any example commands like `glxgears` or `xeyes` or `xclock` to check if the display is correctly connected.

The above information should be sufficient for getting started with using an AWS EC2 instance for running Isaac Gym on your PC. Hope the article helped you with what you wanted. Setup your instance, use Isaac Gym and simulate all your favourite robots!

---
<br>
*This post is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1) by the author.*

