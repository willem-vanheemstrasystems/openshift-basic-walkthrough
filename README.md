# openshift-basic-walkthrough
OpenShift Basic Walkthrough

Based on 'Basic Walkthrough' at https://docs.openshift.com/online/getting_started/basic_walkthrough.html

# Setup

In this section, you will fork the OpenShift Node.js sample application on GitHub and clone the repository to your local machine so that you can deploy and edit the app.

1. On GitHub, navigate to the openshift/nodejs-ex repository. In the top-right corner of the page, click Fork:

2. Next, execute the following commands on your local machine to clone the sample application and change to the new directory:

```javascript
$ git clone https://github.com/willem-vanheemstrasystems/nodejs-ex
$ cd nodejs-ex
```

That’s it! Now, you have a fork of the original openshift/nodejs-ex example application Git repository and a copy on your local machine.

# Creating a New Application

In this section, you will deploy your first application to OpenShift Online using the web console.

1. Navigate to the welcome page of the OpenShift Online web console and click [ New Project ] to create your first project:

If you already have a project, you must delete it in order to continue. The OpenShift Online (Next Gen) Developer Preview only allows you to create a single project at this time.

To delete your existing project, click the trash can icon next to the project name on the welcome page.

***NOTE***: We will be re-using the project 'TotalJS' for now.

2. Replace my-project with a unique name for your project, such as <your_github_username>-example. You can leave the display name and description blank. 

***NOTE***: As we will be re-using the existing project 'TotalJS', skip this step for now.

3. With the project 'TotalJS' selected, click 'Add to Project' then on the 'JavaScript' option.

4. Select the nodejs-mongodb-example Quickstart template: Node.js + MongoDB (Persistent)

5. On the next screen, replace the user name in the Git Repository URL parameter with your GitHub user name (e.g. https://github.com/willem-vanheemstrasystems/nodejs-ex.git). Use the default values provided for all other parameters:

***Name***: ```<leave blank>```

***Namespace***: ```<leave blank>```

***Memory Limit***: 512Mi

***Memory Limit (MongoDB)***: 512Mi

***Volume Capacity***: 1Gi

***Git Repository URL***: https://github.com/willem-vanheemstrasystems/nodejs-ex.git

***Git Reference***: ```<leave blank>```

***Context Directory***: ```<leave blank>```

***Application Hostname***: ```<leave blank>```

***GitHub Webhook Secret***: ```<leave blank>```

***Generic Webhook Secret***: ```<leave blank>```

***Database Service Name***: ```<leave blank>```

***MongoDB Username***: ```<leave blank>```

***MongoDB Password***: ```<leave blank>```

***Database Name***: ```<leave blank>```

***Database Administrator Password***: ```<leave blank>```

***Custom NPM Mirror URL***: ```<leave blank>```

6. Finally, scroll to the bottom of the page and click ***[ Create ]*** to deploy your application.

On the new page, look under ***Applied Parameter Values*** to see all values that have been autogenerated. Make note of these values for later use !

***NOTE***: You can follow along on the ***Overview*** page of the web console to see the new resources being created, and watch the progress of the build and deployment. While the MongoDB pod is being created, its status is shown as pending. The MongoDB pod then starts up and displays its newly-assigned IP address.

# Configuring Automated Builds

In this section, you will configure a GitHub webhook to automatically trigger a rebuild of your application whenever you push code changes to your forked repository. This involves adding the Github webhook URL from your application into your Github repository. Follow the instructions on the page.

The next time you push a code change to your forked repository, your application will automatically rebuild.

# Viewing Your Running Application

In this section, you will view your running application using a web browser.

In the [web console](https://console.preview.openshift.com/console/), view the ***Overview*** page for your project to determine the web address for your application. Click the web address displayed underneath the ***NODEJS-MONGODB-EXAMPLE*** service to open your application in a new browser tab. E.g. http://nodejs-mongo-persistent-totaljs-001.44fs.preview.openshiftapps.com

***NOTE***: You can find all routes configured for your project at any time in the web console:

1. From the web console, navigate to the project containing your application.
2. Click the ***[ Browse ]*** tab, then click ***[ Routes ]***.
3. Click the host name to open your application in a browser new tab.

On the web page of the application, if still the default page, you will see:

```javascript
Request information
Page view count: 1

DB Connection Info:
Type:	MongoDB
URL:	mongodb://172.30.146.136:27017/sampledb
```

The page count will go up everytime somebody opens the page. Its count is stored in the MongoDB database.

We can connect to the MongoDB database, using the URL stated above.

If we want to connect to services (e.g. mongoDB) on OpenShift from outside of OpenShift, we need to do ***port-forwarding*** (read https://blog.openshift.com/getting-started-with-port-forwarding-on-openshift/ ). See also https://docs.openshift.com/online/dev_guide/ssh_environment.html

### Start a Secure Shell Session
Open a remote shell session to a container (after having successfully logged into OpenShift):

```javascript
oc rsh <pod>
```
For 'pod', fill in the name of the pod as shown on the web console, e.g. 'mongodb-1-9iq18' which is a pod that contains a MongoDB instance.

Make connection with the MongoDB, by running the following command:

```javascript
mongo
```

You will be prompted as follows:

```javascript
MongoDB shell version: 3.2.10
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
>
```

Now you can use any MongoDB statements to query the MongoDB instance (see https://docs.mongodb.com/manual/mongo/#start-the-mongo-shell).

***NOTE***: the above database connected to is the 'test' database schema (the default). If you prefer to connect to another database schema (e.g. 'sampledb'), run the following command:

```javascript
> use sampledb
```

You will be prompted as follows:

```javascript
switched to db sampledb
>
```

To authenticate oneself to get access to the database data, run:

```javascript
> db.auth({user: 'userL6K', pwd: '3NX70dFqJ7Qepfl6'})
```

***NOTE***: To obtain the user name and password, echo the following on the Terminal tab of the MongodDB pod instance:

```javascript
echo $MONGODB_USER
echo $MONGODB_PASSWORD
```

While in the remote shell, you can issue commands as if you are inside the container and perform local operations like monitoring, debugging, and using CLI commands specific to what is running in the container.

Get all collections from the sampledb:

```javascript
> db.getCollectionNames()
[ "counts" ]
>
```

It returns one collection, called 'counts' (which in this case stores the page counts).

Look into the collection 'counts' to see what is in there:

```javascript
> db.counts.find()
```

If the page has been visited more than once, it returns documents like so:

```javascript
{ "_id" : ObjectId("58e74cfc2d837200186a9bf2"), "ip" : "10.1.37.1", "date" : 1491553532326 }
{ "_id" : ObjectId("58e76c23984b9100181c5cbd"), "ip" : "10.1.37.1", "date" : 1491561507661 }
{ "_id" : ObjectId("58e77480984b9100181c5cbe"), "ip" : "10.1.37.1", "date" : 1491563648069 }
{ "_id" : ObjectId("58e77a10984b9100181c5cbf"), "ip" : "10.1.37.1", "date" : 1491565072190 }
{ "_id" : ObjectId("58e77da7984b9100181c5cc0"), "ip" : "10.1.37.1", "date" : 1491565991796 }
{ "_id" : ObjectId("58e77db4984b9100181c5cc1"), "ip" : "10.1.37.1", "date" : 1491566004209 }
{ "_id" : ObjectId("58e78ccb984b9100181c5cc2"), "ip" : "10.1.37.1", "date" : 1491569867679 }
>
```

Try refreshing the web page (http://nodejs-mongo-persistent-totaljs-001.44fs.preview.openshiftapps.com/), and execute the db.counts.find() command again. If all goes well a new document will have been added to the collection.

Alternatively, you can have OpenShift connect to external sources (such as databases). See https://docs.openshift.com/online/dev_guide/integrating_external_services.html

If on the web page, at 'Request information, Page view count': is states ***No database configured***, do the following:

# Configuring the MongoDB Database

You may have noticed the index page "Page view count" reads "No database configured". Let's fix that. 

Running ```oc status``` or checking the web console will reveal the ***address*** of the newly created MongoDB:

```javascript
svc/mongodb - 172.30.140.143:27017
  dc/mongodb deploys openshift/mongodb:3.2
    deployment #1 deployed 18 minutes ago - 1 pod

http://nodejs-mongo-persistent-totaljs-001.44fs.preview.openshiftapps.com (svc/nodejs-mongo-persistent)
  dc/nodejs-mongo-persistent deploys istag/nodejs-mongo-persistent:latest <-
    bc/nodejs-mongo-persistent source builds https://github.com/willem-vanheemstrasystems/nodejs-ex.git on openshift/nodejs:4
    deployment #1 deployed 16 minutes ago - 1 pod
```

Note that the url for our new Mongo instance, for our example, is 172.30.140.143:27017, yours will likely differ.

## Setting environment variables

To take a look at environment variables set for each pod, run ```oc env pods --all --list```.

You will be prompted with text similar to the below:

```javascript
# pods mongodb-1-i7fkn, container mongodb
# MONGODB_USER from secret nodejs-mongo-persistent, key database-user
# MONGODB_PASSWORD from secret nodejs-mongo-persistent, key database-password
MONGODB_DATABASE=sampledb
# MONGODB_ADMIN_PASSWORD from secret nodejs-mongo-persistent, key database-admin-password
# pods nodejs-mongo-persistent-1-0gn06, container nodejs-mongo-persistent
DATABASE_SERVICE_NAME=mongodb
# MONGODB_USER from secret nodejs-mongo-persistent, key database-user
# MONGODB_PASSWORD from secret nodejs-mongo-persistent, key database-password
MONGODB_DATABASE=sampledb
# MONGODB_ADMIN_PASSWORD from secret nodejs-mongo-persistent, key database-admin-password
# pods nodejs-mongo-persistent-1-build, container sti-build
BUILD={"kind":"Build","apiVersion":"v1","metadata":{"name":"nodejs-mongo-persistent-1","namespace":"totaljs-001","selfLink":"/oapi/v1/namespaces/totaljs-001/builds/nodejs-mon
go-persistent-1","uid":"7ffa3cdb-1b6a-11e7-b3b6-0e3d364e19a5","resourceVersion":"1094178766","creationTimestamp":"2017-04-07T08:16:28Z","labels":{"app":"nodejs-mongo-persiste
nt","buildconfig":"nodejs-mongo-persistent","openshift.io/build-config.name":"nodejs-mongo-persistent","openshift.io/build.start-policy":"Serial","template":"nodejs-mongo-per
sistent"},"annotations":{"openshift.io/build-config.name":"nodejs-mongo-persistent","openshift.io/build.number":"1"}},"spec":{"serviceAccount":"builder","source":{"type":"Git
","git":{"uri":"https://github.com/willem-vanheemstrasystems/nodejs-ex.git"}},"strategy":{"type":"Source","sourceStrategy":{"from":{"kind":"DockerImage","name":"registry.acce
ss.redhat.com/rhscl/nodejs-4-rhel7@sha256:a7d1f9c3058c197a97eaca339dcbbba4596429df0fb1bcd1578a9ed6f523b2ee"},"env":[{"name":"NPM_MIRROR"}],"forcePull":true}},"output":{"to":{
"kind":"DockerImage","name":"172.30.47.227:5000/totaljs-001/nodejs-mongo-persistent:latest"},"pushSecret":{"name":"builder-dockercfg-y41rw"}},"resources":{},"postCommit":{"sc
ript":"npm test"},"nodeSelector":null,"triggeredBy":[{"message":"Build configuration change"}]},"status":{"phase":"New","outputDockerImageReference":"172.30.47.227:5000/total
js-001/nodejs-mongo-persistent:latest","config":{"kind":"BuildConfig","namespace":"totaljs-001","name":"nodejs-mongo-persistent"}}}

SOURCE_REPOSITORY=https://github.com/willem-vanheemstrasystems/nodejs-ex.git
SOURCE_URI=https://github.com/willem-vanheemstrasystems/nodejs-ex.git
ORIGIN_VERSION=v3.4.1.10
ALLOWED_UIDS=1-
DROP_CAPS=KILL,MKNOD,SETGID,SETUID,SYS_CHROOT
PUSH_DOCKERCFG_PATH=/var/run/secrets/openshift.io/push
```

We need to add the environment variable ***MONGO_URL*** to our Node.js web app so that it will utilize our MongoDB, and enable the "Page view count" feature. Run:

```javascript
oc set env dc/nodejs-mongo-persistent MONGO_URL='mongodb://admin:secret@172.30.140.143:27017/sampledb'
```

Replace 'secret' with MONGODB_ADMIN_PASSWORD

Using:

DATABASE_SERVICE_NAME=mongodb

MONGODB_USER=admin (default)

MONGODB_ADMIN_PASSWORD= see note ->

***NOTE***: To get the MONGODB_ADMIN_PASSWORD, on the running Pod go to the Terminal and ```echo``` the environment variable, like so:

```javascript
echo $MONGODB_ADMIN_PASSWORD
```

You will be promted with something like: Ot4rp5Bs3wHNsDXO, which is the MongoDB admin password

URL TO MONGODB INSTANCE=172.30.140.143:27017

MONGODB_DATABASE=sampledb

You will be prompted with:

```javascript
deploymentconfig "nodejs-mongo-persistent" updated
```

On the web console you will see the application NODEJS MONGO PERSISTENT updating.

You can check if the MONGO_URL has been set, by opening the deployment 'nodejs-mongo-persistent' and look at the tab 'Environment'. It should show a (new) line:

```javascript
MONGO_URL: mongodb://admin:secret@172.30.140.143:27017/sampledb
```

You can check for errors, by looking for the current instance of 'nodejs-mongo-persistent' in the ***Logs*** tab.

For example, if authentication fails you may see: 

```javascript
Server running on http://0.0.0.0:8080
Error connecting to Mongo. Message:
MongoError: Authentication failed.
```

Open the web URL to check if now page counts are being recorded in the database.

## Success

You should now have a Node.js welcome page showing the current hit count, as stored in a MongoDB database.

# Pushing a Code Change

more ...

