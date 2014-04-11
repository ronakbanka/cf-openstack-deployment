cf-openstack-deployment
=======================

The way to a working Openstack deployment with CloudFoundry

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




