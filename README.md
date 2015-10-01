# DevOps Milestone 1

    Divya Jain (djain2)
    Shrenik Gala (sngala)
    Prashant Gupta (pgupta7)

####Target Project
We have used a simple **NodeJs** application for this stage of the project which just responds with a simple message when hit with a request. A test case checks for the string returned, and fails if the string is different than the one in the test case. We have configured **Jenkins** on our machine as our build server.

##Jenkins Configuration

####Plugins Used

- **GitHub Plugin**<br>
    This plugin enables us to use Git as the source code management tool. We have specified the path of the local Git repository in the Repository URL. In 'Branches to build' specifier, we specify the branch name to be built.
- **Email Ext Plugin**<br>
    This plugin helps us to send emails as a post build trigger, on success as well as failure of a build.

##Build section

####Triggered Build

We used a local git repository, and we have added a post-commit hook to each of the developer and release branches to trigger a build for the corresponding job on Jenkins. This is done through the following code in post-commit hook:


    if [ `git rev-parse --abbrev-ref HEAD` = "dev" ]; then
        curl http://localhost:8080/job/dev-build-job/build
    elif [ `git rev-parse --abbrev-ref HEAD` = "release" ]; then
         curl http://localhost:8080/job/release-build-job/build
    fi

Depending on the branch on which the commit happened, the curl command sends a build job notification to Jenkins.

####Dependency Management + Build Script Execution

To execute a build job , we have executed following commands while the job is being executed on Jenkins:

     cd $WORKSPACE
     npm clean
     npm install
     ./script/test

**Note:**
- $WORKSPACE is the environment variable which provides the absolute path of the Jenkins workspace
- npm clean ensures a clean build each time with the dependencies being installed again.

####Build Status + External Post-Build Job Triggers

- The build status can be seen on the Jenkins page, as shown below:

- We have triggered an external event of sending emails on both success and failure of build jobs. We used email-ext plugin for the same and configured it to allow gmail smtp server to send emails to a list of recipients.

####Multiple Branches, Multiple Jobs

This is done through the Git hooks conditions present in the post-commit file. The code is as follows:

    if [ `git rev-parse --abbrev-ref HEAD` = "dev" ]; then
        curl http://localhost:8080/job/dev-build-job/build
    elif [ `git rev-parse --abbrev-ref HEAD` = "release" ]; then
         curl http://localhost:8080/job/release-build-job/build
    fi

The if condition checks the branch in which the commit is made. If the commit is made to the dev branch, the dev build job is triggered otherwise if the commit is made to the release branch , the release build job is triggered.

####Build History and Display over HTTP
    
We can see the build history on our Jenkins server running on: http://localhost:8080
