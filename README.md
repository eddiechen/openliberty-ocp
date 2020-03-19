# openliberty-ocp

This is a quick Openliberty demo on Openshift 4 showing a hello world page and a lil bit of dev/ops

Bring up an Openshift 4.x cluster. You can use RHPDS or your own environment

## On a RHEL server:
  1. yum install -y maven git
  2. git clone https://github.com/eddiechen/openliberty-ocp.git #clone the hello world app
  3. cd openliberty-ocp
  4. mvn clean package (build the app, you should see the target dir)
  5. application is now ready to go
 
 ## On the same RHEL server, install oc:
  1. oc login http://api.<clustername>:6443
  2. oc new-project openliberty-demo-dev
  3. oc create -f https://raw.githubusercontent.com/OpenLiberty/open-liberty-s2i/master/imagestreams/openliberty-ubi-min.json 
          #downloading the latest openliberty container image
  4. 


(Original content from https://edwin.baculsoft.com/2019/11/deploying-a-simple-hello-world-app-using-openliberty-s2i-to-openshift/)
