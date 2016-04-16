# Jenkins
## Setting up Jenkins as a continuous integration server for Django
Jenkins is an easy-to-use open-source continuous integration server. In this post we’ll go through steps needed to set up Jenkins to deploy your Django application and run unit tests whenever someone commits code to your project’s repository. If the new code causes any of your tests to fail, Jenkins will send the commiter an email alert.
### Installing
Jenkins provides packages for most system distributions. Installation is very simple and consists of adding the Jenkins repository to your package system and installing the package. On Ubuntu this can be performed using the following steps:

```
wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

By default Jenkins runs on port 8080 and listens on all network interfaces. After installing the package, you can visit Jenkins under the URL: http://test-server:8080

The default setup provides no security at all, so if your server is accessible outside of a trusted network you will need to secure it. Jenkins documentation describes a basic security setup, which you can extend by proxying Jenkins through your secured Apache or Nginx server, using HTTPs, etc.



