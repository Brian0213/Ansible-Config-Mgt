# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

## PREREQUISITES

- Links to documentation to study:

[Java](https://en.wikipedia.org/wiki/Java)

[.Net](https://en.wikipedia.org/wiki/.NET_Framework)

[Php](https://en.wikipedia.org/wiki/PHP)

[JavaScript](https://en.wikipedia.org/wiki/JavaScript)

- Install Jenkins on the server:

![Ansible Mysql Download](./images/ansible-mysql.PNG)

- Add command to the bash files:

`sudo vi .bash_profile`

![Bash Profile](./images/bash-profile.PNG)

`source ~/.bash_profile`

![Bash Reload](./images/bash-reload.PNG)

- Check Jenkins status

`sudo systemctl start jenkins`

`sudo systemctl enable jenkins`

`sudo systemctl status jenkins`

![Jenkins Success](./images/jenkins-status.PNG)

`sudo systemctl daemon-reload`

![Daemon reload](./images/daemon-reload.PNG)

- To display jenkins initial password:

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

![Jenkins Password](./images/jenkins-initial-pass.PNG)

- Pick the option Install Selected plug-in

![Jenkins Password](./images/jenkins-install-sel.PNG)

- Install & Open Blue Ocean Jenkins Plugin:

![Blue Ocean Install](./images/jenkins-blueocean-install.PNG)

- Create a new pipeline:

- Select GitHub

- Connect Jenkins with GitHub

- Login to GitHub & Generate an Access Tokenhttps://www.darey.io/wp-content/uploads/2021/07/Jenkins-Create-Access-Token-To-Github.png

- Copy Access Token

- Paste the token and connect

- Create a new Pipeline

- At this point you may not have a Jenkinsfile in the Ansible repository, so Blue Ocean will attempt to give you some guidance to create one. But we do not need that. We will rather create one ourselves. So, click on Administration to exit the Blue Ocean console.

- Let us create our Jenkinsfile
Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.

![Deploy Jenkinsfile](./images/deploy-jenkins.PNG)

- Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage:

![Deploy Jenkinsfile Update](./images/deploy-jenkinsf-update.PNG)

- Now go back into the Ansible pipeline in Jenkins, and select configure:

- Update the Build Configuration script path to display the path of the Jenskinsfile:

 ![Deploy Jenkinsfile Path](./images/build-scriptph-deployjenkinsf.PNG)

 - Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile:

  ![Build Configure](./images/build-configure-output.PNG)

  - Back to the pipeline again, this time click "Build now":

  - This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.

To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

- Click on Blue Ocean

![Blueocean Jenkinsfile](./images/blueocean-jenkinsf-output.PNG)

- Select your project

- Click on the play button against the branch:

![Branch Output](./images/branch-output.PNG)

- Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

- Let us see this in action.

- Create a new git branch and name it feature/jenkinspipeline-stages.

- Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub

- To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

- Click on the "Administration" button

- Navigate to the Ansible project and click on "Scan repository now"

- Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.

![New Branch](./images/new-branch.PNG)

![Jenkins File](./images/Jenkinsf-build-test.PNG)

- In GitHub, create a pull and merge, navigate to Jenkins Blueocean and confirm the GitHub actions:

 ![Jenkins File](./images/blueo-main-update1.PNG)

 - A QUICK TASK FOR YOU!

 1. Create a pull request to merge the latest code into the main branch.

2. After merging the PR, go back into your terminal and switch into the main branch.

3. Pull the latest change.

4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
   1. Package 
   2. Deploy 
   3. Clean up
5. Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch

6. Eventually, your main branch should have a successful pipeline like this in blue ocean

Display Jenkins worksapce:

`ls -l /var/lib/jenkins/workspace/`

![Jenkins File](./images/jenkins-workspace.PNG)

![Quick Task](./images/quick-task.PNG)

![Main Output](./images/blueo-main-latest.PNG)

- Clean up output:

![Main Output](./images/cleanup-output.PNG)

### RUNNING ANSIBLE PLAYBOOK FROM JENKINS

Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

1. Installing Ansible on Jenkins

![Ansible Install](./images/ansible-success.PNG)

![Ansible Version](./images/ansible-version.PNG)

- Install Dependencies:

`sudo apt-get install python3-pip`

`python3 -m pip install --upgrade setuptools`

`python3 -m pip install --upgrade pip`

`python3 -m pip install PyMySQL`

`python3 -m pip install mysql-connector-python`

![Dependency Installation](./images/depend-install-5.PNG)

`python3 -m pip install psycopg2==2.7.5 --ignore-installed`

![Dependency Installation](./images/depend-install-6.PNG)

-Install Postgres Sql Database:

![Postgresql Installation](./images/postgresql-install.PNG)

2. Installing Ansible plugin in Jenkins UI:

![Postgresql Installation](./images/ansible-jenkins-install.PNG)

3. Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)
You can watch a 10 minutes video here to guide you through the entire setup

Note: Ensure that Ansible runs against the Dev environment successfully.

Possible errors to watch out for:

Ensure that the git module in Jenkinsfile is checking out SCM to main branch instead of master (GitHub has discontinued the use of Master due to Black Lives Matter. You can read more here)

Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set.

![Jenkins Build Documentation](https://wiki.jenkins.io/display/JENKINS/Building+a+software+project)

- Create anisble.cfg file inside the deploy folder

![Ansible Cfg file](./images/ansible-cfg-1.PNG)

![Ansible Cfg file](./images/ansible-cfg-2.PNG)

- Run ansible on jenkins:

![Ansible Cfg file](./images/prepare-ansible-execution.PNG)

![Parameters Jenkins Field Implementation](./images/build-parameters-jenkins .PNG)

![Jenkins Playbook Success](./images/jenkins-success1.PNG)

![Jenkins Playbook Success](./images/jenkins-success2.PNG)

![Jenkins Playbook Success](./images/jenkins-success3.PNG)

![Jenkins Playbook Success](./images/jenkins-success4.PNG)

![Jenkins Playbook Success](./images/jenkins-success5.PNG)

![Jenkins Playbook Success](./images/jenkins-success6.PNG)

![Jenkins Playbook Success](./images/jenkins-success7.PNG)

#### CI/CD PIPELINE FOR TODO APPLICATION

- We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

Our goal here is to deploy the application onto servers directly from Artifactory rather than from git. If you have not updated Ansible with an Artifactory role, simply use this guide to create an Ansible role for Artifactory (ignore the Nginx part). Configure Artifactory on Ubuntu 20.04:

- Phase 1 – Prepare Jenkins
Fork the repository below into your GitHub account:

`https://github.com/darey-devops/php-todo.git`

-Clone the todo repo into the instance server:

- Cd out of the Ansible Config Mgt:

![Change into the Instance](./images/cd-out-ansible-config.PNG)

`git clone https://github.com/Brian0213/php-todo.git`

![Clone Success](./images/clone-success.PNG)

- Add php-todo folder to the Workspace

- On you Jenkins server, install PHP, its dependencies and Composer tool (Feel free to do this manually at first, then update your Ansible accordingly later):

`sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}`

![Php Install Output](./images/php-install-output.PNG)

- Install composer:

`curl -sS https://getcomposer.org/installer | php`

![Composer Install](./images/composer-success.PNG)

- Move the composer into its bin

`sudo mv composer.phar /usr/bin/composer`

`composer --version`

![Composer Install](./images/composer-move-version.PNG)

- Install Jenkins plugins
  1. Plot plugin
  2.Artifactory plugin
 - We will use plot plugin to display tests reports, and code coverage information.
 - The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server.

 ![Plot Install](./images/jenkins-plot-install.PNG)

 ![Artifactory Install](./images/artifactory-install.PNG)

 - In Jenkins UI configure Artifactory:

 - Launch the Artifactory Red Hat instance

 - Add the Instance Ip to the ci file inside the inventory folder:

 ![Artifactory Ip](./images/artifactoryip-in-ci.PNG)

- Ensure the artifactory.yml file is created in the static-assignments folder.

- Ensure artifactory folder is in the roles folder:

- Run artifcatory ansible playbook:

- In the Jesnkins UI, navigate to the project branch and change the build parameters to ci (where the artifactory server is store)

![Build Parameter](./images/build-params.PNG)

- Click the Build button:

- You should get the screenshots below:

![Jenkins Artifactory](./images/jenkins-art-success.PNG)

![Jenkins Artifactory](./images/jenkins-art-lst.PNG)

![Jenkins Artifactory](./images/jenkins-art-last.PNG)

Copy the Artifactory public ip and launch on a browser using port 8081:

[Arifactory Url](http://3.144.121.186:8082/ui/login/)

![Artifactory Url Output](./images/artif-url-output.PNG)

- Onboard in the jfrog website:

![Jfrog Onboarding](./images/jfrog-onboard.PNG)

- Create a repo:

![Jfrog Repo Creation](./images/repo-success.PNG)

- In Jenkins UI configure Artifactory:

![Jenkins Artifactory UI](./images/jenkins-artif-setup.PNG)

Phase 2 – Integrate Artifactory repository with Jenkins

- Create a dummy Jenkinsfile in the repository

- Using Blue Ocean, create a multibranch Jenkins pipeline

- On the database server, create database and user

- To check if the database was created:

- ssh into the database instance:

![Jenkins Artifactory UI](./images/db-mysql-success1.PNG)

![Jenkins Artifactory UI](./images/db-mysql-success2.PNG)

Create a new job in Blueocean:

- Log into the database server:

- Change the bind-address:

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

![Bind Address Update](./images/bind-address-update.PNG)

![MySql Status](./images/mysql-status.PNG)

- Update the .env.sample file with infomation below:

'DB_CONNECTION=mysql
DB_PORT=3306"

Update the DB_Host with the Database instance private ip.

- To connect the instance mysql:

`mysql -h 172.31.44.235 -u homestead -p`

![My Sql Instance Connect](./images/instance-mysql-connect.PNG)

![My Sql Instance Connect](./images/instance-mysql-connect2.PNG)

- Notice the Prepare Dependencies section

The required file by PHP is .env so we are renaming .env.sample to .env
Composer is used by PHP to install all the dependent libraries used by the application
php artisan uses the .env file to setup the required database objects – (After successful run of this step, login to the database, run show tables and you will see the tables being created for you)

!BlueOcean Php-to Build](./images/blueo-php-build.PNG)

- Update the Jenkinsfile to include Unit tests step and run the build again in Jenkins:

![Build Output](./images/build-output1.PNG)

![Build Output](./images/build-output1.PNG)

- Phase 3 – Code Quality Analysis
This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc. Read the article here for more

The data produced by phploc can be ploted onto graphs in Jenkins.

- Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.

- Plot the data using plot Jenkins plugin.
This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc.

![Plot Coverage](./images/plot-code-coverage.PNG)

- Verify that the plot icon displays in the main branch

![Jenkins Plot Icon](./images/ploticon-in-main.PNG)

- Bundle the application code for into an artifact (archived package) upload to Artifactory

- Publish the resulted artifact into Artifactory

 ![Artifact to Artifactory](./images/artifact-to-artifactory.PNG)

- Log into the artifactory/jfrog server to confirm deployment of php-todo

![Artifact to Artifactory](./images/artifactory-deploy-success.PNG)

- Deploy the application to the dev environment by launching Ansible pipeline:

Run ansible against todo instance

![Ansible Todo Instance](./images/ansible-todo-success1.PNG)

![Ansible Todo Instance](./images/ansible-todo-success2.PNG)

#### SONARQUBE INSTALLATION
Before we start getting hands on with SonarQube configuration, it is incredibly important to understand a few concepts:

Software Quality – The degree to which a software component, system or process meets specified requirements based on user needs and expectations.
Software Quality Gates – Quality gates are basically acceptance criteria which are usually presented as a set of predefined quality criteria that a software development project must meet in order to proceed from one stage of its lifecycle to the next one.
SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as The Sonar Way). Software testers and developers would normally work with project leads and architects to create custom quality gates.

- Tune Linux Kernel
This can be achieved by making session changes which does not persist beyond the current session terminal.

`sudo sysctl -w vm.max_map_count=262144`

`sudo sysctl -w fs.file-max=65536`

`ulimit -n 65536`

`ulimit -u 4096`

![Tunnel Linux Output](./images/tunnel-linux.PNG)

- To make a permanent change, edit the file /etc/security/limits.conf and append the below:

`sudo nano /etc/security/limits.conf`

![Tunnel Linux Output](./images/etc-secu-limits.PNG)

- Before installing, let us update and upgrade system packages:

`sudo apt-get update`

![Apt Get Update](./images/apt-get-update.PNG)

`sudo apt-get upgrade`

![Apt Get Upgrade](./images/apt-get-upgrade.PNG)

- Install wget and unzip packages

`sudo apt-get install wget unzip -y`

![Apt Get Wget](./images/apt-get-wget.PNG)

- Install OpenJDK and Java Runtime Environment (JRE) 11

`sudo apt-get install openjdk-11-jdk -y`

![Openjdk 11](./images/openjdk-11.PNG)

`sudo apt-get install openjdk-11-jre -y`

![Openjdk 11](./images/openjdk-11-jre.PNG)

- Set default JDK – To set default JDK or switch to OpenJDK enter below command:

`sudo update-alternatives --config java`

![Java Update](./images/java-update.PNG)

`java -version`

![Java Version](./images/java-vers-output.PNG)

- Install and Setup PostgreSQL 10 Database for SonarQube

The command below will add PostgreSQL repo to the repo list:

`sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'`

![Postsgre repo](./images/postgre-repo.PNG)

- Download PostgreSQL software:

`wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -`

![Postsgre Download](./images/download-ok.PNG)

- Install PostgreSQL Database Server:

`sudo apt-get -y install postgresql postgresql-contrib`

![Postsgre Database Server](./images/postgre-db-server.PNG)

- Start PostgreSQL Database Server:

`sudo systemctl start postgresql`

- Enable it to start automatically at boot time:

`sudo systemctl enable postgresql`

![Postsgre Start Enable](./images/postgre-start-enable.PNG)

- Change the password for default postgres user (Pass in the password you intend to use, and remember to save it somewhere):

`sudo passwd postgres` (desh)

![Postsgre Start Enable](./images/postgre-start-enable.PNG)

Switch to the postgres user:

`su - postgres`

![Postsgre User](./images/su-postgres.PNG)

Create a new user by typing:

`createuser sonar`

![Sonar Create](./images/sonar-creat.PNG)

- Switch to the PostgreSQL shell:

`psql`

![Switch PG Shell](./images/psql.PNG)

- Set a password for the newly created user for SonarQube database:

`ALTER USER sonar WITH ENCRYPTED password 'sonar';`

![Switch PG Shell](./images/psql.PNG)

- Create a new database for PostgreSQL database by running:

`CREATE DATABASE sonarqube OWNER sonar;`

![Create Database Sonar](./images/db-sonar.PNG)

- Grant all privileges to sonar user on sonarqube Database:

`grant all privileges on DATABASE sonarqube to sonar;`

![Grant Database Sonar](./images/grant-sonar.PNG)

- Exit from the psql shell:

`\q`

- Switch back to the sudo user by running the exit command:

`exit`

![Quit Exit](./images/quit-exit.PNG)

- Install SonarQube on Ubuntu 20.04 LTS:

- Navigate to the tmp directory to temporarily download the installation files:

`cd /tmp && sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.3.zip`

![Tmp files](./images/tmp-files.PNG)

- Unzip the archive setup to /opt directory:

`sudo unzip sonarqube-7.9.3.zip -d /opt`

![Zip files](./images/zip-files.PNG)

Move extracted setup to /opt/sonarqube directory:

`sudo mv /opt/sonarqube-7.9.3 /opt/sonarqube`

![Tmp Mv](./images/tmp-mv.PNG)

#### CONFIGURE SONARQUBE
We cannot run SonarQube as a root user, if you run using root user it will stop automatically. The ideal approach will be to create a separate group and a user to run SonarQube

- Create a group sonar:

`sudo groupadd sonar`

- Now add a user with control over the /opt/sonarqube directory:

`sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar`

`sudo chown sonar:sonar /opt/sonarqube -R`

![600 -606](./images/sonar-add-dir.PNG)

- Open SonarQube configuration file using your favourite text editor (e.g., nano or vim):

`sudo vim /opt/sonarqube/conf/sonar.properties`

Find the following fines for user name :

#sonar.jdbc.username=
#sonar.jdbc.password=

Uncomment them and provide the values of PostgreSQL Database username and password:

![User](./images/user-pass-update.PNG)

- Edit the sonar script file and set RUN_AS_USER:

`sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh`

![Edit Sonar Script](./images/edit-sonar-script.PNG)

- Now, to start SonarQube we need to do following:

- Switch to sonar user:

`sudo su sonar`

![Sonar Su](./images/su-sonar.PNG)

- Move to the script directory:

`cd /opt/sonarqube/bin/linux-x86-64/`

- Run the script to start SonarQube:

`./sonar.sh start`

Expected output shall be as:

![Script Run](./images/script-run.PNG)

- Check SonarQube running status:

`./sonar.sh status`

Sample Output below:

![Output](./images/output.PNG)

- To check SonarQube logs, navigate to /opt/sonarqube/logs/sonar.log directory:

`tail /opt/sonarqube/logs/sonar.log`

![Output](./images/output.PNG)

Output:

![Output](./images/logs-output.PNG)


##### Configure SonarQube to run as a systemd service

- Stop the currently running SonarQube service:

`cd /opt/sonarqube/bin/linux-x86-64/`

- Run the script to start SonarQube:

`./sonar.sh stop`

![Sonar Sh Stop](./images/sonarsh-stop.PNG)

- Create a systemd service file for SonarQube to run as System Startup:

`./sonar.sh stop`

![Sonar Sh Stop](./images/sonarsh-stop.PNG)

- Change to root

`sudo su`

- Create a systemd service file for SonarQube to run as System Startup.

`sudo nano /etc/systemd/system/sonar.service`

- Add the configuration below for systemd to determine how to start, stop, check status, or restart the SonarQube service.

![System Sonar output](./images/systemd-sonar-output.PNG)

- Save the file and control the service with systemctl

`sudo systemctl start sonar`

`sudo systemctl enable sonar`

`sudo systemctl status sonar`

![Save System Status](./images/start-enable-status.PNG)

#####  Access SonarQube

- To access SonarQube using browser, type server’s IP address followed by port 9000:

[Sonar Ip](http://3.17.166.34:9000/projects)

![Sonar Url](./images/sonar-url-output.PNG)

- Now, when SonarQube is up and running, it is time to setup our Quality gate in Jenkins.

###### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE

In Jenkins, install SonarScanner plugin:

![Sonar Scanner](./images/sonar-scan-install.PNG)

Navigate to configure system in Jenkins. Add SonarQube server as shown below:

Manage Jenkins > Configure System

![Sonar Jenkins Install](./images/sonar-jenkins-install.PNG)

- Generate authentication token in SonarQube:

![Sonar Token](./images/sonar-token.PNG)

- Configure Quality Gate Jenkins Webhook in SonarQube – The URL should point to your Jenkins server:

[Sonar Jenkins Hook Url](http://{JENKINS_HOST}/sonarqube-webhook/)

![Sonar Jenkins Hook Output](./images/sonar-jenkins-hook.PNG)

- Setup SonarQube scanner from Jenkins – Global Tool Configuration:

![Sonar Scanner Config](./images/sonar-scanner.PNG)

###### Update Jenkins Pipeline to include SonarQube scanning and Quality Gate

- Below is the snippet for a Quality Gate stage in Jenkinsfile.

![Sonar Scanner Config](./images/sonar-scanner.PNG)

NOTE: The above step will fail because we have not updated `sonar-scanner.properties

Configure sonar-scanner.properties – From the step above, Jenkins will install the scanner tool on the Linux server. You will need to go into the tools directory on the server to configure the properties file in which SonarQube will require to function during pipeline execution.

`cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/`

![Cd Tools](./images/cd-to-tools.PNG)

- Open sonar-scanner.properties file

`sudo vi sonar-scanner.properties`

- Add configuration related to php-todo project:

![Cd Tools](./images/update-conf.PNG)

- End-to-End Pipeline Overview

Indeed, this has been one of the longest projects from Project 1, and if everything has worked out for you so far, you should have a view like below:

##### End-to-End Pipeline Overview

Indeed, this has been one of the longest projects from Project 1, and if everything has worked out for you so far, you should have a view like below:

![Sonar Success](./images/sonar-success1.PNG)

![Sonar Success](./images/sonar-success2.PNG)

But we are not completely done yet!

The quality gate we just included has no effect. Why? Well, because if you go to the SonarQube UI, you will realise that we just pushed a poor-quality code onto the development environment.

Navigate to php-todo project in SonarQube:

![Sonar Phptodo](./images/sonar-phptodo.PNG)

- There are bugs, and there is 0.0% code coverage. (code coverage is a percentage of unit tests added by developers to test functions and objects in the code)

If you click on php-todo project for further analysis, you will see that there is 6 hours’ worth of technical debt, code smells and security issues in the code.

Complete the following tasks to finish Project 14
Introduce Jenkins agents/slaves – Add 2 more servers to be used as Jenkins slave. Configure Jenkins to run its pipeline jobs randomly on any available slave nodes.
Configure webhook between Jenkins and GitHub to automatically run the pipeline when there is a code push.









- Install the postgresql:

`sudo ansible-galaxy collection install community.postgresql`

![Ansible Todo Instance](./images/postgres-install.PNG)

