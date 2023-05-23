# Intro

These are a collection of scripts and instructions to leverage cloud computing platforms for NGS analysis (like RNA-seq). The instructions provided here are for the Google Cloud Platform. If you don't have an account yet, you can [sign up for a free trial](https://cloud.google.com/free) where you will get $300 credit (or $400 for Wits email) for 3 months. 

---

## Table of contents

---

## Google Cloud Platform (GCP)

### Create account and activate google cloud platform

Create a Google account if you don't already have one and then go to https://console.cloud.google.com/ to setup your cloud account. 

---
### Creating a storage bucket

Think of a storage bucket as a cloud storage drive where you can save your files. You can use it as a shared folder to transfer files to and from your local computer to a VM. 

1. From the [GCP Dashboard](https://console.cloud.google.com/home/dashboard), click on the menu button, then click on **Storage**. 

2. This will open the storage browser and list any buckets you have (similar to the VM Instances page. Click **Create Bucket** at the top.

3. This will open a new page where you must enter details for your new bucket.

![Image of GCP Dashboard](Screenshots/GCP/createbucket-1.png)

  + You must specify a globally unique name (meaning the name must not be taken by anyone else)
  + For **Location type**, select Region
  + For **Location**, select the **same region as the VM instances you have created!**
  + Leave all other settings as defaults and click **Create** at the bottom.

4. Your newly created bucket will be listed. Click on its name to view its contents (which will be empty right now).

5. You can upload files by using the buttons provided or by dragging files into the browser from your computer.

---

### Creating a compute instance

1. From the [GCP Dashboard](https://console.cloud.google.com/home/dashboard), click on the menu button (1), then hover over **Computer Engine** to display the pop-up menu (2), then click on **VM instances** (3)
  ![Image of GCP Dashboard](Screenshots/GCP/createinstance.png)
  
2. This is where all of your existing VMs will be listed. To create a new VM, click **Create Instance** at the top.
  ![Image of GCP Dashboard](Screenshots/GCP/createinstance-2.png)

3. The page will ask you to specify details for your new VM instance. Notice the cost right. The montly cost is the cost you will incur if you run the VM 24/7. You won't be doing this for analysis purposes; you will only run it for a few hours so only pay attention to the hourly cost which is 6.1 cents/hour for the default configuration shown here. The more powerful your VM, the faster tasks will finish (generally speaking), so in some situations it would make sense to get a more expensive hourly VM. 
![Image of GCP Dashboard](Screenshots/GCP/createinstance-3.png)
  You need to specify these details:
  + Instance name
  + Region and zone: Select **us-east1** for Region because it is closest to you and the cheapest cost.
  + Machine configuration: select a prebuilt configuration or customize your own CPU and memory allotment. 
  + Click the **Change** button under **Boot disk** to specify the hard drive and OS configuration.
![Image of GCP Dashboard](Screenshots/GCP/createinstance-4.png)
    + Select Ubuntu for the **Operating system**
    + Select Ubuntu 18.04 LTS for **Version**
    + Set the size for 500 GB
  + Under **Identity and API access**, select **Allow full access to all Cloud APIs**
  + Under **Firewall**, select **Allow HTTP traffic** and **Allow HTTPS traffic**
  + Click the **Create** button at the button when you are done.

4. This will return you to the **VM instances** page where you will see your newly created instance. The green icon indicates the VM is currently running.
![Image of GCP Dashboard](Screenshots/GCP/createinstance-5.png)

5. To connect to the VM, click on the **SSH** button (after ensuring it is on). You can use the menu button to Start, Stop, Reset, or Delete a VM.


### How to setup a newly created VM

#### Setting up GCSFuse
A newly created VM only has a basic install of Ubuntu and you must now install any software you need. 

The first step is to install [gcsfuse](https://github.com/GoogleCloudPlatform/gcsfuse) on your new VM. Gcsfuse allows you to connect your VM to a storage bucket you own. This will "mount" your storage bucket as a virtual drive in your VM so you can transfer files to and from it. 

Copy and paste these commands to setup gcsfuse:
  ```
  #install gcsfuse
  export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s`
  echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
  curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  sudo apt-get update
  sudo apt-get install gcsfuse
  #create folder for mounting
  mkdir mountfolder
  ```

You now have gcsfuse installed and have created a folder called "mountfolder". This is where you will mount your storage bucket.

Enter this command to mount your storage bucket:
  ```
  gcsfuse myBucketName mountfolder
  ```
  Where ```myBucketName``` is the name of the storage bucket you created.

---
#### Install Miniconda

Miniconda is required to download and install some bioinformatics tools. You can install it using these commands:
  ```
  #download setup install script
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  #run setup
  bash Miniconda3-latest-Linux-x86_64.sh
  ```
  Then follow the prompts to complete installation.
  
You may have to open a new terminal window in order for the ```conda``` command to be recognized.

Try entering this command:
  ```
  conda --version
  ```
  You should see the version number of conda if it installed correctly.

---
#### Installing Basemount

Like gcsfuse, basemount has their own software to let you interface with your basepsace account. This will allow you to mount your basespace account as a virtual drive and transfer your read files.

Enter these commands to install basemount:
  ```
  #install basemount
  sudo bash -c "$(curl -L https://basemount.basespace.illumina.com/install)"
  #create folder for mounting
  mkdir basemountfolder
  ```
  
You now have basemount installed and have created a folder called "basemountfolder". This is where you will mount your basemount account.

Enter this command to mount your storage bucket:
  ```
  basemount basemountfolder
  ```
  You will now be prompted to login to your basespace account and once you do, you can access your files.

---
### Install VNC server

You may also wish to have  graphical user interface (GUI) desktop for your server instead of a command-line only ssh terminal interface. To do this, you must install a VNC (virtual network computing) server that will allow you to remotely connect to a desktop. You must also install a desktop environment on your Linux VM because one is not installed by default.

#### Automatically install VNC server on Ubuntu

A script is provided to setup a VNC server and automatically configure it. Download the required files from here: 

1. Download the setup files here: https://github.com/developerpiru/cloudservers/blob/master/VNC-setup.tar.gz
2. Copy this to your storage bucket that you created above
3. Mount your storage bucket with gcsfuse if you haven't done so already:
  ```
  gcsfuse myBucketName mountfolder
  ```
4. Navigate to where you copied vnc-setup.zip in your storage bucket
  ```
  cd mountfolder/path/to/where/you/saved/the/file
  ```
5. Extract the VNC-setup.tar.gz archive
	```
	tar -xzvf VNC-setup.tar.gz
	```
6. Change directories into the newly extracted folder
	```
	cd VNC-setup
	```
7. Run the script:
	```
	bash vnc-setup.sh
	```
---	
#### Install VNC client on your local computer

You need a VNC client to connect to the VNC server you just setup on your VM. 

You can install either PuTTY for Windows (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) or RealVNC's VNC Viewer for Windows, Mac OS, or Linux (https://www.realvnc.com/en/connect/download/viewer/)

See below for instructions on how to connect to your remote server.

---
#### Connecting to VNC server on remote server
1. On your VM, start the VNC server with this command:
	```
	vncserver -geometry 1200x1050
	```
	**Note:** the `-geometry 1200x1050` flag is optional and you can customize it to any resolution you prefer

2. Open your VNC client (PuTTY or VNC Viewer) and enter the **external IP address** (not its local IP!) of your remote computer followed by the `:5901` port. For example: `192.0.2.0:5901`. You can find the external IP address of your VM by looking at your [VM instances list](https://console.cloud.google.com/compute/instances).
	
3. Enter the password you created earlier when you ran the vnc-setup.sh script.

4. If prompted, select to use the default desktop configuration.

You should now see the Ubuntu desktop now and be able to interact with the GUI using your mouse and keyboard. 

**Note:** to help you with downloading any other required components, it is recommended that you install Chrome or Firefox on your Linux VM:

Chrome:
	
	wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
	sudo dpkg -i google-chrome-stable_current_amd64.deb
	
	
Firefox:
	
	sudo apt-get update
	sudo apt-get install firefox
	

#### Stopping the VNC server
You can stop the VNC server by entering this command in a terminal on your VM:
	
	vncserver -kill :1
	
---



