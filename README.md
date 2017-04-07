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

***Memory Limit***: 512Mi

***Memory Limit (MongoDB)***: 512Mi

***Volume Capacity***: 1Gi

***Git Repository URL***: https://github.com/willem-vanheemstrasystems/nodejs-ex.git

***Git Reference***: ```<leave blank>```

6. Finally, scroll to the bottom of the page and click ***[ Create ]*** to deploy your application.

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

If on the web page, at 'Request information, Page view count': is states ***No database configured***, do the following:

# Configuring the MongoDB Database

more ...

# Pushing a Code Change

more ...

