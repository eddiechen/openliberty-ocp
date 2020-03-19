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
          #_downloading the latest openliberty container image_
  4. oc new-build --name=openliberty-demo --image-stream=openliberty:19.0.0.6 --binary=true
  5. oc start-build openliberty-demo --from-dir=.    #_make sure you are in the openliberty-ocp directory_
     #You can monitor the progress in the OCP dashboard
        * select developer mode
        * select project openliberty-demo-dev
        * Builds -> openliberty-demo -> builds -> check status and look for complete
  6. oc tag openliberty-demo:latest openliberty-demo:1.0
  7. oc get is  #_verify the tag is in place_
  8. oc new-app openliberty-demo --name=openliberty-demo-dev
  9. oc expose svc/openliberty-demo-dev
 10. You will need to modify the route
        * on Openshift dashboard, switch to administrator view
        * networking -> routes -> openliberty-demo-dev -> yaml
        * There are 2 lines that look like this example "openliberty-demo-dev-openliberty-demo-dev.apps.cluster-vfc-fec4.vfc-           fec4.example.opentlc.com"
        * change it to example like this "openliberty-demo-dev.apps.cluster-vfc-fec4.vfc-fec4.example.opentlc.com"
        * save
 11. Go to web browser and you should be able to see the following page
        * http://openliberty-demo-dev.apps.<openshift cluster link>/com.edw-1.0-SNAPSHOT/hello.jsp
 12. You have successfully deployed an app in your development project
  
  ## Deploying the app to production
   1. On the same RHEL server as above
   2. oc tag  openliberty-demo:latest  openliberty-demo:prod
   3. oc new-project openliberty-demo-prod
   4. oc new-app --image-stream=openliberty-demo-dev/openliberty-demo:prod
   5. oc expose svc/openliberty-demo
   6. You will need to modify the route
        * on Openshift dashboard, switch to administrator view
        * networking -> routes -> openliberty-demo-prod -> yaml
        * There are 2 lines that look like this example "openliberty-demo-openliberty-demo-prod.apps.cluster-vfc-fec4.vfc-           fec4.example.opentlc.com"
        * change it to example like this "openliberty-demo-prod.apps.cluster-vfc-fec4.vfc-fec4.example.opentlc.com"
        * save
  


(Original content from https://edwin.baculsoft.com/2019/11/deploying-a-simple-hello-world-app-using-openliberty-s2i-to-openshift/)
