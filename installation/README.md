# Installin Spinnaker 

### Instruction 

<p>You can find Installation steps on below given link </p>

[click_here](https://spinnaker.io/docs/setup/install/)

## prerequisite 

<img src="pre.png">

### complete steps to install Spinnaker 

<img src="complete.png">

### installation Resources 

<ul>
    <li> local machine or VM </li>
    <li> can be cloud providers </li>
    <li> using docker and kubernetes </li>

</ul>


## Installing Spinnaker on Ubuntu Vm 

### what we need 

<ol>
     <li> Ubuntu 18.04 or Higher </li>
     <li> 16GB RAM & 4 Core Cpu along with 50GB storage </li>
     <li> halyard -- the spinnaker installer  </li>
     <li> storage -- i am using s3 -- use can use any other options link is given below </li> 
</ol>

### storage options 

[click_to_know](https://spinnaker.io/docs/setup/install/storage/)

## steps 

### step 1 -- creating user for spinnaker 

```
root@ip-172-31-23-80:~# adduser spinnaker 
Adding user `spinnaker' ...
Adding new group `spinnaker' (1001) ...
Adding new user `spinnaker' (1001) with group `spinnaker' ...
Creating home directory `/home/spinnaker' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for spinnaker
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
root@ip-172-31-23-80:~# vim /etc/sudoers.d/90-cloud-init-users 
root@ip-172-31-23-80:~# cat /etc/sudoers.d/90-cloud-init-users 
# Created by cloud-init v. 22.2-0ubuntu1~20.04.1 on Mon, 29 Aug 2022 03:23:42 +0000

```

### adding user to sudoers 

```
# User rules for ubuntu
ubuntu ALL=(ALL) NOPASSWD:ALL
spinnaker ALL=(ALL) NOPASSWD:ALL
root@ip-172-31-23-80:~# 
```

###  step 2 -- Install halyard 

```
root@ip-172-31-23-80:~# su - spinnaker 
spinnaker@ip-172-31-23-80:~$ 
spinnaker@ip-172-31-23-80:~$ curl -O https://raw.githubusercontent.com/spinnaker/halyard/master/install/debian/InstallHalyard.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7898  100  7898    0     0   104k      0 --:--:-- --:--:-- --:--:--  104k

====
spinnaker@ip-172-31-23-80:~$ ls
InstallHalyard.sh
spinnaker@ip-172-31-23-80:~$ sudo bash InstallHalyard.sh 
Halyard version will be 1.50.0 
Halyard will be downloaded from the spinnaker-community repository 
Halconfig will be stored at /home/spinnaker/.hal/config
Uninstall script is located at /usr/local/bin/uninstall-halyard.sh
Installing Java...
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://us-eas

```

### checking halyard installation 

```
spinnaker@ip-172-31-23-80:~$ hal -v
1.50.0
```

### step 3 -- choose cloud providers to provision spinnaker components 

### note: i am not using any cloud provider so all my spinnaker components will be running in this vm only 

### step 4 -- choosing Environment 

<p> check for documentation </p>

[click](https://spinnaker.io/docs/setup/install/environment/)

### i am using localdebian ENV 

```
spinnaker@ip-172-31-23-80:~$ hal config deploy edit --type localdebian
+ Get current deployment
  Success
+ Get the deployment environment
  Success
- No changes supplied.

```

### step 5 -- choosing storage -- i am using amazon s3 

### Note: i am already having a bucket so i am giving path -- bucket has versioning enabled 

```
spinnaker@ip-172-31-23-80:~$ hal config storage s3 edit  --access-key-id  AKIA25YZWFYDXARPLVNN  --secret-access-key --region us-east-2  --bucket  ashutoshhspinnaker 
Your AWS Secret Key.: 
+ Get current deployment
  Success
+ Get persistent store
  Success
+ Edit persistent store
  Success
Validation in default.persistentStorage:
- WARNING Your deployment will most likely fail until you configure
  and enable a persistent store.

Validation in default:
- WARNING You have not yet selected a version of Spinnaker to
  deploy.
? Options include: 
  - 1.28.1
  - 1.27.1
  - 1.26.7
  - 1.25.7
  - 1.24.6
  - 1.23.7

+ Successfully edited persistent store "s3".
```

### Note: if you don't have bucket created then only use 

```
hal config storage s3 edit  --access-key-id  AKIA25YZWFYDXARPLVNN  --secret-access-key --region us-east-2
```

### change storage type to s3 

```
spinnaker@ip-172-31-23-80:~$ hal config storage edit --type s3
+ Get current deployment
  Success
+ Get persistent storage settings
  Success
+ Edit persistent storage settings
  Success
Validation in default:
- WARNING You have not yet selected a version of Spinnaker to
  deploy.
? Options include: 
  - 1.28.1
  - 1.27.1
  - 1.26.7
  - 1.25.7
  - 1.24.6
  - 1.23.7

+ Successfully edited persistent storage.
```

