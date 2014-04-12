Openstack Deploymnet
=======================
The way to a working Openstack deployment with CloudFoundry

Preinstallation
====
Preinstallation setup routine:
````bash
sudo apt-get install mercurial
sudo apt-get install bzr

bash < <(curl -s https://raw.github.com/moovweb/gvm/master/binscripts/gvm-installer)
source /home/ubuntu/.gvm/scripts/gvm

gvm install go1.1.1
gvm use go1.1.1
mkdir ~/go
export GOPATH=~/go
export PATH=$PATH:$GOPATH/bin
go get github.com/vito/spiff
````

Merging the files above:
````bash
spiff merge cf-deployment.yml cf-jobs.yml cf-networks.yml cf-properties.yml cf-settings.yml > deployment.yml 
````

Running deployment:
````bash
bosh deployment deployment.yml
bosh deploy
````

Currently struggeling with:
API deploymment does not work. Log says that there is 'bad content', currently not sure what is causing the issue. Nfs did not launch last deployment, because the volume was not created. 
Last run resultet in running version of the deployment, though the resulting URLs:
- http://api.mydomain.xipio -> NOT FOUND (possibly due to misconfiguration with https)
- http://login.mydomain.xipio -> NOT FOUND (possibly due to misconfiguration with https)
- http://console.mydomain.xipio -> NOT FOUND (possibly due to misconfiguration with https)
- http://uaa.mydomain.xipio -> FOUND, but JDBC Database error.

Please contribute!

Debugging with SSH
===
Connection to a bosh instance for debugging via SSH. This can be done like this:

````bash
ubuntu@micro-bosh:~$ bosh ssh
1. nats_z1/0
2. postgres_z1/0
3. uaa_z1/0
4. login_z1/0
5. runner_z1/0
6. loggregator_z1/0
7. loggregator_trafficcontroller_z1/0
8. router_z1/0
Choose an instance: 8
Enter password (use it to sudo on remote host): **********
Target deployment is `cf-cs'

Setting up ssh artifacts
Please specify a public key file
````

In some cases, mostly on newly setup Ubuntus you will recieve the error above `Please specify a public key file` which can be easily solved via:

````bash
ubuntu@micro-bosh:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
f0:0c:84:4e:c6:a6:3c:7a:35:c5:92:f9:47:f9:94:4b ubuntu@micro-bosh
The key's randomart image is:
+--[ RSA 2048]----+
|   . =.  . .     |
|    X.o o E      |
| . * +o. + .     |
|  + + .=. o      |
| . o . .S        |
|. .              |
| .               |
|                 |
|                 |
+-----------------+
````

Afterwards you should be able to connect to the instance without any issue.
````bash
ubuntu@micro-bosh:~$ bosh ssh
1. nats_z1/0

...

8. router_z1/0
Choose an instance: 8
Enter password (use it to sudo on remote host): **********
Target deployment is `cf-cs'

Setting up ssh artifacts

Director task 132

Task 132 done
Starting interactive shell on job router_z1/0
The authenticity of host '172.24.100.130 (172.24.100.130)' can't be established.
RSA key fingerprint is 89:8a:85:ff:83:f2:4a:d6:52:08:61:0c:c6:7c:9c:25.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.24.100.130' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ubuntu/.ssh/id_rsa': 
Linux e2afe694-4164-4012-aca8-600e47d01e15 3.0.0-32-virtual #51~lucid1 SMP Thu Mar 6 17:43:24 UTC 2014 x86_64 GNU/Linux
Ubuntu 10.04.4 LTS

....

bosh_ti5pu6jn6@e2afe694-4164-4012-aca8-600e47d01e15:~$ 

````
