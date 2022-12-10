# Documentation for Project 12: ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

## Step 1 – Jenkins job enhancement

- Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.

- Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

`sudo mkdir /home/ubuntu/ansible-config-artifact`

![Ansible Artifcat](./images/ansible-artifact.PNG)

- Change permissions to this directory, so Jenkins could save files there:

`sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact`

![Change Ansible Artifcat](./images/chmod-ansible-artifact.PNG)

- Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins:

![Install Copy Artifcat](./images/install-copy-artifact.PNG)

- Create a new Freestyle project (you have done it in Project 9) and name it save_artifacts:

![Project Save Artifcat](./images/save-artifact.PNG)

- This project will be triggered by completion of your existing ansible project. Configure it accordingly:

- Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

- Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

- If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

Now your Jenkins pipeline is more neat and clean.

![Ansible Save Artifcat](./images/ansible-save-artifact.PNG)

### REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML

- Step 2 – Refactor Ansible code by importing other playbooks into site.yml

- Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

- Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

- Move common.yml file into the newly created static-assignments folder.

- Inside site.yml file, import common.yml playbook.

- The code above uses built in import_playbook Ansible module.

- Your folder structure should look like this;

![Folder Structure](./images/folder-structure.PNG)

- Run ansible-playbook command against the dev environment:

- Since you need to apply some tasks to your dev servers and wireshark is already installed – you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.

- update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:

![Update Site.yml File](./images/update-site-yml.PNG)

![Inventory Path](./images/inventory-path.PNG)

- Add the inventory path to the inventory in the ansible configuration and uncomment ansible:

`sudo vi /etc/ansible/ansible.cfg`

![Ansible Inventory Path](./images/ansible-inventory-path.PNG)

- Run the command below to check the status of the servers:

`ansible all -m ping`

![Ansible Ping other Servers](./images/ansible-ping-servers.PNG)

- Change back the folder:

![Change Directory Back](./images/cd-dir.PNG)

- Run this command:

`ansible-playbook -i inventory/dev.yml playbooks/site.yml`

![](./images/cd-dir.PNG)

- Make sure that wireshark is deleted on all the servers by running wireshark --version

`wireshark --version`


### CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’

Step 3 – Configure UAT Webservers with a role ‘Webserver’

- We have our nice and clean dev environment, so let us put it aside and configure 2 new Web Servers as uat. We could write tasks to configure Web Servers in the same playbook, but it would be too messy, instead, we will use a dedicated role to make our configuration reusable.

- Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UAT.

- Tip: Do not forget to stop EC2 instances that you are not using at the moment to avoid paying extra. For now, you only need 2 new RHEL 8 servers as Web Servers and 1 existing Jenkins-Ansible server up and running.

- To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.

- Create the directory/files structure manually
Note: You can choose either way, but since you store all your codes in GitHub, it is recommended to create folders and files there rather than locally on Jenkins-Ansible server.

The entire folder structure should look like below, but if you create it manually – you can skip creating tests, files, and vars or remove them if you used ansible-galaxy

- After removing unnecessary directories and files, the roles structure should look like this:

![Folder Output](./images/folder-struct.PNG)

- Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers
NOTE: Ensure you are using ssh-agent to ssh into the Jenkins-Ansible instance just as you have done in project 11:

![Uat Yml Output](./images/uat-yml.PNG)

4.In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles:

`sudo vi /etc/ansible/ansible.cfg`

![Ansible Config RolePath](./images/ansible-config-rolepath.PNG)

5.It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

- Install and configure Apache (httpd service)
- Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
-Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
- Make sure httpd service is started:
Your main.yml may consist of following tasks:

![Task Main Yml](./images/task-main-yml.PNG)

#### REFERENCE WEBSERVER ROLE

Step 4 – Reference ‘Webserver’ role

- Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml. This is where you will reference the role:

![Static Assignment New File](./images/static-webs.PNG)

- Remember that the entry point to our ansible configuration is the site.yml file. Therefore, you need to refer your uat-webservers.yml role inside site.yml.

- So, we should have this in site.yml:

![Site Yml Update](./images/siteyml-update.PNG)