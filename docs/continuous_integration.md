# Continuous Integration (CI)

Continuous integration is the practice of frequently merging code changes.

But frequent merges cause difficulties:
- What if the code doesn't compile after a merge
- What if someone broke something
- It would be a lof of work to check these things on every merge

The solution is to automate this process.

## How CI works ?
We use a CI server which executes a build that automatically prepares/compiles the code and runs automated tests.

Usually, the CI server automatically detects code changes in source control and run the build whenever there is a change

If something is wrong, the build fails. The team can get immediate feedback if their change broke something. It can take in form of emails.

Merging frequently throughout the day and getting quick feedback means that if you broke something, you broke it with your changes within the last couple hours. This is much easier to identify/fix than if you broke something with your changes from a week ago!

We will be using Jenkins as our CI server.

## Setting up

Let's set up Jenkins:
```console
sudo yum -y remove java
sudo yum -y install java-1.8.0-openjdk
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum -y install jenkins-2.121.1
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Go to <your server address>:8080 in a browser,
in my case, it is http://10.211.55.4:8080/
and on the home page, you will get this message:
```console
Unlock Jenkins
To ensure Jenkins is securely set up by the administrator, a password has been written to the log (not sure where to find it?) and this file on the server:
/var/lib/jenkins/secrets/initialAdminPassword
Please copy the password from either location and paste it below.
```

Thus on the server, copy/paste the password required that you can find in /var/lib/jenkins/secrets/initialAdminPassword

Then, choose the default setting for the set up

Register the adminstrator account:
Username: jenkins
password: jenkins

After that, login to <your server address>:8080
in my case, http://10.211.55.4:8080.
In the case, there is an error, type the url <your server address>:8080/restart
  
 

