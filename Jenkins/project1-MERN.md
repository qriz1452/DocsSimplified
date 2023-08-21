======== MERN JENKINS PROJECT ==================

https://www.freecodecamp.org/news/automate-mern-app-deployment-with-jenkins/
My Repo : **https://github.com/qriz1452/productivity-app-jenkins-mern-project.git**

================================================

Step 1: 

launch servers
in server 1 install jenkins and java 
in server 2 install java 
https://www.jenkins.io/doc/book/installing/linux/


------------

Step 2 : 


login to jenkins and initial setup
add server 2 as node for jobs
https://docs.google.com/document/d/1QW8P5OGjMUAu27IjXj8g_i8aX4Mo4uA0eEqrNnGc-y0/edit#heading=h.ok3kiklvnjvp
...... steps in lab number : sr5.lab4


-------------------


step 3: 

Fork git app code repo
Main repo : https://github.com/itsrakeshhq/productivity-app
Forked Repo  : https://github.com/qriz1452/productivity-app-jenkins-mern-project

---------------------


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





----------------------

----------------------

