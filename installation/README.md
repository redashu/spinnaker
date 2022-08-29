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

### step 6 : -- Deploy spinnaker using halyard 

### check versions of halyard 

```
spinnaker@ip-172-31-23-80:~$ hal version list
+ Get current deployment
  Success
+ Get Spinnaker version
  Success
+ Get released versions
  Success
+ You are on version "", and the following are available:
 - 1.23.7 (Hemlock Grove):
   Changelog: https://gist.github.com/spinnaker-release/94280a2b615adccd975eed73359023ac
   Published: Fri Feb 19 01:25:30 UTC 2021
   (Requires Halyard >= 1.32.0)
 - 1.24.6 (Queen's Gambit):
   Changelog: ht
```

### set any stable version 

```
spinnaker@ip-172-31-23-80:~$ hal config version edit --version 1.28.1
+ Get current deployment
  Success
+ Edit Spinnaker version
  Success
+ Spinnaker has been configured to update/install version "1.28.1".
  Deploy this version of Spinnaker with `hal deploy apply`.
```

### Now deploy spinnaker -- may take 15 to 25 minutes -- depends on internet speed 

```
spinnaker@ip-172-31-23-80:~$ sudo  hal deploy apply 
+ Get current deployment
  Success
_ Prep deployment
  Running validation: Validating stats with PrometheusCanaryAccountValidator

```

### checking ports 

```
spinnaker@ip-172-31-23-80:~$ netstat -nlpt
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp6       0      0 127.0.0.1:8087          :::*                    LISTEN      19429/java          
tcp6       0      0 127.0.0.1:8088          :::*                    LISTEN      19427/java          
tcp6       0      0 127.0.0.1:8089          :::*                    LISTEN      19424/java          
tcp6       0      0 127.0.0.1:7002          :::*                    LISTEN      19423/java          
tcp6       0      0 :::8064                 :::*                    LISTEN      15266/java          
tcp6       0      0 ::1:6379                :::*                    LISTEN      -                   
tcp6       0      0 127.0.0.1:8080          :::*                    LISTEN      19425/java          
tcp6       0      0 127.0.0.1:8083          :::*                    LISTEN      19428/java          
tcp6       0      0 127.0.0.1:8084          :::*                    LISTEN      19426/java          
tcp6       0      0 :::22                   :::*                    LISTEN      -  
```

### changing configuration to access it from external 

```
root@ip-172-31-23-80:/opt/spinnaker/config# head -20 spinnaker.yml 
## WARNING
## This file was autogenerated, and _will_ be overwritten by Halyard.
## Any edits you make here _will_ be lost.

services:
  clouddriver:
    host: localhost
    port: 7002
    baseUrl: http://localhost:7002
    enabled: true
  deck:
    host: localhost
    port: 9000
    baseUrl: http://0.0.0.0:9000
    enabled: true
```
### make some changes in apache2 configuration 

#### change 1 

```
root@ip-172-31-23-80:~# cd /etc/apache2/
root@ip-172-31-23-80:/etc/apache2# ls
apache2.conf    conf-enabled  magic           mods-enabled  ports.conf       sites-enabled
conf-available  envvars       mods-available  passphrase    sites-available
root@ip-172-31-23-80:/etc/apache2# vim ports.conf 
root@ip-172-31-23-80:/etc/apache2# cat ports.conf 
## WARNING
## This file was autogenerated, and _will_ be overwritten by Halyard.
## Any edits you make here _will_ be lost.

Listen 0.0.0.0:9000

<IfModule ssl_module>
  SSLPassPhraseDialog exec:/etc/apache2/passphrase
</IfModule>
root@ip-172-31-23-80:/etc/apache2# 

```

#### change 2 

```
root@ip-172-31-23-80:/etc/apache2# ls
apache2.conf    conf-enabled  magic           mods-enabled  ports.conf       sites-enabled
conf-available  envvars       mods-available  passphrase    sites-available
root@ip-172-31-23-80:/etc/apache2# cd sites-enabled/
root@ip-172-31-23-80:/etc/apache2/sites-enabled# ls
spinnaker.conf
root@ip-172-31-23-80:/etc/apache2/sites-enabled# ls -l spinnaker.conf 
lrwxrwxrwx 1 root root 33 Aug 29 04:06 spinnaker.conf -> ../sites-available/spinnaker.conf
root@ip-172-31-23-80:/etc/apache2/sites-enabled# vim spinnaker.conf 
root@ip-172-31-23-80:/etc/apache2/sites-enabled# cat spinnaker.conf 
## WARNING
## This file was autogenerated, and _will_ be overwritten by Halyard.
## Any edits you make here _will_ be lost.

<VirtualHost 0.0.0.0:9000>
  <IfModule ssl_module>
    SSLEngine on
    SSLCertificateFile ""
    SSLCertificateKeyFile ""
  </IfModule>
  DocumentRoot /opt/deck/html

  <Directory "/opt/deck/html/">
     Require all granted
  </Directory>
</VirtualHost>

```

### step 7 -- restarting all the services 

```
 sudo systemctl restart apache2
 sudo systemctl restart gate
sudo systemctl restart orca
sudo systemctl restart igor
 sudo systemctl restart front50
 sudo systemctl restart echo
 sudo systemctl restart clouddriver
 sudo systemctl restart rosco
```

### access it using IP:9000 -- handled by deck 

