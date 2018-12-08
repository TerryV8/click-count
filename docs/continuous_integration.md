# Continuous Integration (CI)

Continuous integration is the practice of frequently merging code changes.

But frequent merges cause difficulties:
- What if the code doesn't compile after a merge
- What if someone broke something
It would be a lof of work to check these things on every merge

The solution is to automate this process.

### How it works ?
We use a CI server which executes a build that automatically prepares/compiles the code and runs automated tests.

Usually, the CI server automatically detects code changes in source control and run the build whenever there is a change

If something is wrong, the build fails. The team can get immediate feedback if their change broke something. It can take in form of emails.

Merging frequently throughout the day and getting quick feedback means that if you broke something, you broke it with your changes within the last couple hours. This is much easier to identify/fix than if you broke something with your changes from a week ago!

For that, we will be using Jenkins as our CI server.

## Setting up CI server

To set up Jenkins:
```console
sudo yum -y remove java
sudo yum -y install java-1.8.0-openjdk
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum -y install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Go to <your server address>:8080 in a browser,
eg. http://10.211.55.4:8080/

On the home page, you got this message:
```console
Unlock Jenkins
To ensure Jenkins is securely set up by the administrator, a password has been written to the log (not sure where to find it?) and this file on the server:
/var/lib/jenkins/secrets/initialAdminPassword
Please copy the password from either location and paste it below.
```
Follow the instruction, with the default setting

You can use as adminstrator account:
Username: jenkins
password: jenkins

In the case, there is an error, type the url <your server address>:8080/restart
  
 
### Setting up Jenkins Projects
Jenkins projects are at the core of doing any kind of automation in Jenkins. 
A Jenkins project is the configuration which controls what a piece of Jenkins automation does and when it executes.

We will demonstrate how to set up a freestyle project that pulls application source code from GitHub and executes build automation against it. 

Go your Jenkins homepage,
click on "New Item"
Enter the name for the jenkins project
Then choose "Freestyle project" as a project type
Now I am ready to set up my project to pull the source code from git.
From the source code management, select Git and paste the url from gitHub. I don't need to set up any crudential because this git repository is publicly readable.

Now, I go ahead and implementing my build step. Click on add build step
When I finish, click on "Build now"

If I click on console output, I can watch the console output from the build himself
and see that CI is currently in the process of running

This is the output expected in the console:
```console

```

# Automatically trigerring builds as soon as commit to git

For my project, I haven't set up the automatic triger yet.
Beliw I am going to give you the idea on how to configure it.

Some teams have a scheduled builds that runs one a week, one a day, twice a day.
However, the most efficient and immediate way is to have a trigger.

We are going to use Webhooks which is event notifications made from one application to another over http.

In jenkins, we can use webhooks to have Github notify Jenkins as soon as the code in GitHub changes.
Jenkins can respond by automatically running the build to implement any changes.

We can configure Jenkins to automatically create and manage webhooks in GitHub.
So we must give jenkins access to an API token that allows to access the gitHub API.

Configurin webhooks in Jenkins is realatively easy. We need to:
- Create an access token in GitHub that has permission to read and create webhooks
- Add a GitHub server in Jenkins for GitHub.com
- Create a jenkins credential with the token and configure the GitHub server configuration to use it
- Check "Manage Hooks" for the GitHub server configuration
- In the project configuration, under "Build triggers", select "GitHub hook trigger for GITScm polling"

### On GitHub
click Settings >> Developer settings >> Personal access tokens >> Generate new token
Select the right permission: admin:repo_hook >> write:repo_hook and read:repo_hook

### On Jenkins
Click Manage Jenkins >> Configure system
In the section GitHub, select Git server, name: GitHub, Credentials: Add jenkins
Select Kind: Secret text, Secret: "Enter the api key of GitHub", ID: github_key, Description: GitHubKey
Then Credential: GitHubKey, check "Manage hooks", click on "test connection"

I ve just set up my Jenkins server to be able to authenticate with GitHub.

Click on the project >> Configure. 
- In the section "Source Code Management" >> Git >> Repository URL: https://github.com/TerryV8/cicd-pipeline-train-schedule-jenkins
- In the section  "Build triggers", select "Github hook trigger for GITScm polling"

# Jenkins pipelines

A jenkins pipeline is an automated process built on these tools. It takes source code through a "pipeline" from the source code creation all the way to production deployment.

Pipelines adhere to the best practice of infrastructure as code. Therefore, a pipeline is implemented in a file that is kept in source control along with the rest of the application code. This file is called a Jenkinsfile.
To create a pipeline, simply create a file called Jenkinsfile and add it to you source control repo.
When creating the Jenkins project, chosse the "pipeline" or "Multibranch pipeline" project type.

Pipelines has a domain-specific-language(DSL) that is used to define the pipeline logic

There are 2 styles of Pipeline syntax you can choose:
- either scripted - a bit more like procedural code
- either declarative - syntax describes the pipeline logic


On Github, create a new item pipepline
select your project >> Configure >> Build Triggers >> 


# workflow

1. Code locally on a feature branch
2. Open a pull request on Github against the master branch
3. Run automated tests against the Docker container
4. If the tests pass, manually merge the pull request into master
5. Once merged, the automated tests run again
6. If the second round of tests pass, a build is created on Docker Hub
7. Once the build is created, it’s then automatically (err, automagically) deployed to production



![workflow](https://files.realpython.com/media/steps.91fb3b3eef5a.jpg)


# How to run Jenkins under a different user in Linux [Redhat]
When I wanted to change the Jenkins user I first checked /etc/init.d/jenkins script.  There I found two important variables $JENKINS_CONFIG(=/etc/sysconfig/jenkins) and $JENKINS_USER. So if you want, you can change the JENKINS_USER variable in the /etc/init.d/jenkins file; but it is not the correct way to do.

To change the jenkins user, open the /etc/sysconfig/jenkins (in debian this file is created in /etc/default) and change the JENKINS_USER to whatever you want. Make sure that user exists in the system (you can check the user in the /etc/passwd file ).
$JENKINS_USER="manula"
Then change the ownership of the Jenkins home, Jenkins webroot and logs.
chown -R manula:manula /var/lib/jenkins 
chown -R manula:manula /var/cache/jenkins
chown -R manula:manula /var/log/jenkins
Then restarted the Jenkins jenkins and check the user has changed using a ps command 
/etc/init.d/jenkins restart
ps -ef | grep jenkins


