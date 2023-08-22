======== MERN JENKINS PROJECT ==================

https://www.freecodecamp.org/news/automate-mern-app-deployment-with-jenkins/

My Repo : **https://github.com/qriz1452/productivity-app-jenkins-mern-project.git** 

================================================

Step 1: 
```
launch servers
in server 1 install jenkins and java 
in server 2 install java 
https://www.jenkins.io/doc/book/installing/linux/
```

------------

Step 2 : 

```
login to jenkins and initial setup
add server 2 as node for jobs
https://docs.google.com/document/d/1QW8P5OGjMUAu27IjXj8g_i8aX4Mo4uA0eEqrNnGc-y0/edit#heading=h.ok3kiklvnjvp
...... steps in lab number : sr5.lab4
```

-------------------


step 3: 
```
Fork git app code repo
Main repo : https://github.com/itsrakeshhq/productivity-app
Forked Repo  : https://github.com/qriz1452/productivity-app-jenkins-mern-project
```

---------------------

```
STEP 4 :
Frontend code in : client
backed code in : server
install git in both master and slave
start writing piepile  ( later checkout out to forked repo )
install node and npm in both machines

There are  few environment variables in `activity.test.js `  of backedn directory i.e server so add these variables in jenkins

install docker on master  : 
https://docs.docker.com/engine/install/ubuntu/

and add jenkins user in docker s it can access docker ``` sudo systemctl restart jenkins ```

```



----------------------

----------------------


JENKINS CINFIGURATION :

Firewall :
for debian based it is ufw and for other it is firewalld or firewall-cmd

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


installed :
```
Warnings Next Generation ,
Lockable Resources plugin,
Throttle Concurrent Builds plugin. .
 copy artifacts  ,
 Locale Plugin,
 Performance Version  ,
 pipeline graph view,
 pipeline stage view,
 blue ocean ,
  Configuration as Code .,
  thin backup ,
  

    Dark Theme Plugin - provides a dark theme for Jenkins. Supports configuration as code to select the theme configuration.

    Material Theme Plugin - port of Afonso F’s Jenkins material theme to use Theme Manager.

    Solarized Theme Plugin - provides Solarized (light and dark) themes.
Simple Theme Plugin.

login  Theme Plugin.


See Git userContent plugin for how to manage these files through a Git repository.
 Job Restrictions Plugin



```
 



Restrict project naming : unable to find this option .,
jmeter installation is by linux commands ,




sites : 
```
http://yourserver/jenkins/threadDump


    BUILD ID URL/stop - aborts a Pipeline.

    BUILD ID URL/term - forcibly terminates a build (should only be used if stop does not work).

    BUILD ID URL/kill - hard kill a pipeline. This is the most destructive way to stop a pipeline and should only be used as a last resort.
To see the time zone currently set, go to jenkins_server/systemInfo
".../api/" URL where "..." portion is the data that it acts on.
/restart and /safeRestart
 navigate to JENKINS_URL/me/configure

This feature can be accessed from "Manage Jenkins" > "Script Console".  Or by visiting the sub-URL /script on your Jenkins instance.

$JENKINS_HOME/userContent, and these files are served from http://yourhost/jenkins/userContent.




```

=== securing jenkins afterwords remaining===
