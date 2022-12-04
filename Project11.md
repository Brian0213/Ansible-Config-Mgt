# Documentation for Project 11: ANSIBLE – AUTOMATE PROJECT 7 TO 10

## Preparing prerequisites

- Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible

- In your GitHub account create a new repository and name it ansible-config-mgt

### INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

- Install Ansible

`sudo apt update`

![Apt Update](./images/apt-update.PNG)

`sudo apt install ansible -y`

![Ansible Install](./images/ansible-install.PNG)

- Check your Ansible version by running:

`ansible --version`

![Ansible Version](./images/ansible-version.PNG)

- Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

- Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

- Configure Webhook in GitHub and set webhook to trigger ansible build.

- Configure a Post-build job to save all (**) files, like you did it in Project 9.

![Github Webhook](./images/github-webhook-configure.PNG)

![Jenkins Ist Build](./images/jenkins-1st-build-output.PNG)

- Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder:

![Jenkins Update Build](./images/jenkins-update-output.PNG)

- To check:

`sudo ls /var/lib/jenkins/jobs/ansible/builds`

![Sudo Jenkins Jobs](./images/var-lib-jenkins.PNG)

`sudo ls /var/lib/jenkins/jobs/ansible/builds/2/archive/`

![Sudo Jenkins Jobs](./images/display-build.PNG)

- Change to the archive folder

`cd /var/lib/jenkins/jobs/ansible/builds/2/archive/`

![Change to Archive](./images/cd-ls-archive.PNG)

- Display Readme file content:

`cat README.md`

![Readme Content](./images/cat-readme.PNG)

- Tip:  Every time you stop/start your Jenkins-Ansible server – you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server (you have done it before to your LB server in Project 10). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

#### Prepare your development environment using Visual Studio Code

- First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – Visual Studio Code (VSC), you can get it here.

- In VS Code, install Remote Development.

- After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.

- Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

`git clone <ansible-config-mgt repo link>`

##### BEGIN ANSIBLE DEVELOPMENT

- In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.

- Tip: Give your branches descriptive and comprehensive names, for example, if you use Jira or Trello as a project management tool – include ticket number (e.g. PRJ-145) in the name of your branch and add a topic and a brief description what this branch is about – a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm).

- Checkout the newly created feature branch to your local machine and start building your code and directory structure

![New Branch](./images/new-branch.PNG)

- Create a directory and name it playbooks – it will be used to store all your playbook files:

[Playbooks](C:\Users\oluse\OneDrive\Desktop\Oluwasegun-workspace\Ansible-Config-Mgt\playbooks)

- Create a directory and name it inventory – it will be used to keep your hosts organised

[Inventory](C:\Users\oluse\OneDrive\Desktop\Oluwasegun-workspace\Ansible-Config-Mgt\inventory)

- Within the playbooks folder, create your first playbook, and name it common.yml:

![Common Yml File](./images/playbook-common.PNG)

- Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively:

![Inventory Files](./images/inventory-files.PNG)

###### Set up an Ansible Inventory

- An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

- Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

- N.B. Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent:

`eval `ssh-agent -s`

![Eval Ssh Agent](./images/eval-ssh-agent.PNG)

`ssh-add <path-to-private-key>`

- Confirm the key has been added with the command below, you should see the name of your key:

`ssh-add -l`

Now, ssh into your Jenkins-Ansible server using ssh-agent:

`ssh -A ubuntu@public-ip`

![Jenkins Ansible start](./images/jenkins-ansible-start.PNG)

- To persist the pem key:

`ssh-add -l`

-Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

- Update your inventory/dev.yml file with this snippet of code:

![Inventory Devyml](./images/inventory-devyml-update.PNG)

###### CREATE A COMMON PLAYBOOK

- Create a Common Playbook

- It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

- In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

- Update your playbooks/common.yml file with following code:

![Commonyml Update](./images/commonyml-update.PNG)

- Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

Feel free to update this playbook with following tasks:

Create a directory and a file inside it
Change timezone on all servers
Run some shell script

- For a better understanding of Ansible playbooks – watch this video from RedHat and read this article.

[Introduction to Ansible Playbook Video](https://www.youtube.com/watch?v=ZAdJ7CdN7DY)

[Introduction to Ansible Playbook Article](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook)

###### Update GIT with the latest code

- Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

- In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes – it is also called "Four eyes principle".

- Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

- Commit your code into GitHub:

- use git commands to add, commit and push your branch to GitHub:

`git commit`

`git push`

![Git Commit & Push](./images/prj11-commit-push.PNG)

![Git Pull & Merge](./images/git-pull-merge.PNG)

![Jenkins AutoUpdate](./images/jenkins-autobuild-update.PNG)

- Once your code changes appear in main branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

`sudo ls /var/lib/jenkins/jobs/ansible/builds/3/archive`

`cd /var/lib/jenkins/jobs/ansible/builds/3/archive`

`cd inventory`

`ls`

`cat dev.yml`

![Line 189 - 197 Output](./images/server-archive-info.PNG)

###### RUN FIRST ANSIBLE TEST

- Step 7 – Run first Ansible test

- Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

- Connect to server to VS code

![VS Code](./images/server-connect-vscode.PNG)

`ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/5/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/5/archive/playbooks/common.yml`
