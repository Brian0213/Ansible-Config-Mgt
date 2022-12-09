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

![Change Directory Back](./images/cd-dire.PNG)
