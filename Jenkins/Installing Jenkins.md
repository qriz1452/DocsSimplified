Jenkins is typically run as a standalone application in its own process. The Jenkins WAR file bundles Winstone, a Jetty servlet container wrapper, and can be started on any operating system or platform with a version of Java supported by Jenkins.

 Jenkins can also be run as a servlet in a traditional servlet container like Apache Tomcat or WildFly, but in practice this is largely untested and there are many caveats. 

 --------

 Docker :

 If you are installing Docker on a Linux-based operating system, ensure you configure Docker so it can be managed as a non-root user.
 
There are several Docker images of Jenkins available.

Use the recommended official jenkins/jenkins image from the Docker Hub repository. This image contains the current Long-Term Support (LTS) release of Jenkins, which is production-ready. However, this image doesn’t contain Docker CLI, and is not bundled with the frequently used Blue Ocean plugins and its features. If we want these features we can make a new image.


 Jenkins home directory (/var/jenkins_home) 
 

If you specified the --volume jenkins-data:/var/jenkins_home option in the docker run …​ command, access the contents of the Jenkins home directory through your container’s terminal/command prompt using the docker container exec command:

docker container exec -it <docker-container-name> bash


Kubernetes (k8):


For setting up a Jenkins Cluster on Kubernetes, we will do the following:

    Create a Namespace

    Create a service account with Kubernetes admin permissions.

    Create local persistent volume for persistent Jenkins data on Pod restarts.

    Create a deployment YAML and deploy it.

    Create a service YAML and deploy it.

 Kubernetes manifest files : git clone https://github.com/scriptcamp/kubernetes-jenkins

 all the steps available on docs,
 

In this Jenkins Kubernetes deployment we have used the following:

    'securityContext' for Jenkins pod to be able to write to the local persistent volume.

    Liveness and readiness probe to monitor the health of the Jenkins pod.

    Local persistent volume based on local storage class that holds the Jenkins data path '/var/jenkins_home'.



HELM (k8) :
A typical Jenkins deployment consists of a controller node and, optionally, one or more agents. To simplify the deployment of Jenkins, we’ll use Helm to deploy Jenkins. Helm is a package manager for Kubernetes and its package format is called a chart. Many community-developed charts are available on GitHub.


hard steps .....


YAML File (k8) :
total new

Jenkins operator (k8) :


The Jenkins Operator is a Kubernetes native Operator which manages operations for Jenkins on Kubernetes.



Linux :



On Debian and Debian-based distributions like Ubuntu you can install Jenkins through apt.
Beginning with Jenkins 2.335 and Jenkins 2.332.1, the package is configured with systemd rather than the older System V init



The package installation will:

    Setup Jenkins as a daemon launched on start. Run systemctl cat jenkins for more details.

    Create a ‘jenkins’ user to run this service.

    Direct console log output to systemd-journald. Run journalctl -u jenkins.service if you are troubleshooting Jenkins.

    Populate /lib/systemd/system/jenkins.service with configuration parameters for the launch, e.g JENKINS_HOME

    Set Jenkins to listen on port 8080. Access this port with your browser to start configuration



If Jenkins fails to start because a port is in use, run systemctl edit jenkins and add the following:

[Service]
Environment="JENKINS_PORT=8081"

Here, "8081" was chosen but you can put another port available


Fedora :

You can install Jenkins through dnf. You need to add the Jenkins repository from the Jenkins website to the package manager first.





If you have a firewall installed, you must add Jenkins as an exception. You must change YOURPORT in the script below to the port you want to use. Port 8080 is the most common.

```
YOURPORT=8080
PERM="--permanent"
SERV="$PERM --service=jenkins"

firewall-cmd $PERM --new-service=jenkins
firewall-cmd $SERV --set-short="Jenkins ports"
firewall-cmd $SERV --set-description="Jenkins port exceptions"
firewall-cmd $SERV --add-port=$YOURPORT/tcp
firewall-cmd $PERM --add-service=jenkins
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```

Red Hat/Alma/Rocky :

You can install Jenkins through yum on Red Hat Enterprise Linux, Alma Linux, Rocky Linux, Oracle Linux, and other Red Hat based distributions.


Macos :
 Jenkins can be installed using the Homebrew package manager. Homebrew formula: jenkins-lts This is a package supported by a third party which may be not as frequently updated as packages supported by the Jenkins project directly. 

 
windows :

The simplest way to install Jenkins on Windows is to use the Jenkins Windows installer. That program will install Jenkins as a service using a 64 bit JVM chosen by the user. Keep in mind that to run Jenkins as a service, the account that runs Jenkins must have permission to login as a service.


FreeBSD :

Jenkins can be installed on FreeBSD using the standard FreeBSD package manager, pkg.

`systemctl` not available hence we need to use `service`


 WAR file :
 

The Jenkins Web application ARchive (WAR) file bundles Winstone, a Jetty servlet container wrapper, and can be started on any operating system or platform with a version of Java supported by Jenkins. See the Java Requirements page for details.

Run the WAR file

The Jenkins Web application ARchive (WAR) file can be started from the command line like this:

    Download the latest Jenkins WAR file to an appropriate directory on your machine

    Open up a terminal/command prompt window to the download directory

    Run the command java -jar jenkins.war

    Browse to http://localhost:8080 and wait until the Unlock Jenkins page appears

    Continue on with the Post-installation setup wizard below



Notes:

    This process does not automatically install any specific plugins. They need to installed separately via the Manage Jenkins > Plugins page in Jenkins.

    You can change the port by specifying the --httpPort option when you run the java -jar jenkins.war command. For example, to make Jenkins accessible through port 9090, then run Jenkins using the command:
    java -jar jenkins.war --httpPort=9090

    You can change the directory where Jenkins stores its configuration with the JENKINS_HOME environment variable. For example, to place the Jenkins configuration files in a subdirectory named my-jenkins-config, define JENKINS_HOME=my-jenkins-config before running the java -jar jenkins.war command. Use the Windows commands:


help :  java -jar jenkins.war --help


Tomcat 9:


Jenkins requires Servlet API 4.0 (Jakarta EE 8) with javax.servlet imports. Jenkins is incompatible with Servlet API 5.0 (Jakarta EE 9) or later with jakarta.servlet imports. Ensure that the Servlet API version of your chosen servlet container is compatible before running Jenkins.




Tomcat 9 is based on Servlet API 4.0 (Jakarta EE 8), which is the version of the servlet API required by Jenkins.



Jenkins can be deployed to Tomcat by placing the Jenkins WAR file in the ${CATALINA_HOME}/webapps/ directory.

To configure the Jenkins home directory, set the JENKINS_HOME Java system property via the CATALINA_OPTS environment variable. For example, create ${CATALINA_HOME}/bin/setenv.sh with the following contents:

```export CATALINA_OPTS=-DJENKINS_HOME=/var/lib/jenkins```


WildFly 26:


WildFly 26 is based on Servlet API 4.0 (Jakarta EE 8), which is the version of the servlet API required by Jenkins.



Jenkins can be deployed to WildFly by placing the Jenkins WAR file in the ${JBOSS_HOME}/standalone/deployments/ directory.

To configure the Jenkins home directory, set the JENKINS_HOME Java system property via the JAVA_OPTS environment variable. For example, update ${JBOSS_HOME}/bin/standalone.conf with the following contents:

```JAVA_OPTS="$JAVA_OPTS -DJENKINS_HOME=/var/lib/jenkins"```

 Initial Settings :
 IMP IMP IMP very long  --> https://www.jenkins.io/doc/book/installing/initial-settings/
 


--------














