# How to Backup and Restore Jenkins

[https://medium.com/@_oleksii_/how-to-backup-and-restore-jenkins-complete-guide-62fc2f99b457](https://medium.com/@_oleksii_/how-to-backup-and-restore-jenkins-complete-guide-62fc2f99b457)

![](https://miro.medium.com/max/1400/1*LOFbTP2SxXcFpM_qTsUSuw.png)

Jenkins is an open source automation server written in Java. Jenkins helps to automate the non-human part of the software development process, with continuous integration and facilitating technical aspects of continuous delivery.
Setting access rights, selecting the necessary plugins and jobs configuration is quite a laborious process, so it’s a good idea to organize regular backups of all the necessary settings and parameters.

In this article, we will consider the process of setting up a backup and restore Jenkins to a new server step by step.

## Configure the regular backup job

### Step 1. Create a new Jenkins Job

You should choose Freestyle project.

![](https://miro.medium.com/max/1400/1*kl51a9rxEzcnkbnpvfrYMQ.png)

### Step 2. Mark “None” for Source Control Management

![](https://miro.medium.com/max/1400/1*fMsUdrHMUAaP8kRyHhQEfA.png)

### Step 3. Add a backup job

Let’s add a backup job which will completely backup Jenkins including all jobs, playbooks, whatever else you have there. It will backup everything located at `$JENKINS_HOME`. Select the **“Build Periodically”** build trigger and configure to run as frequently as you like.

![](https://miro.medium.com/max/1400/1*1alKkHSiHCgpEeUI00SP7Q.png)

### Step 4. Add a new “Execute Shell” build step

Add a new “Execute Shell” build step and add the content of [this file](https://gist.github.com/alexsplashex/b996054561ebf9e2e234782e88941d8c) as the command.

![](https://miro.medium.com/max/5784/1*wXp72Y7BSYRDhNqkVsAgxg.png)

### Step 5. Save all changes

![](https://miro.medium.com/max/1400/1*yzkYuxWRV1KPYffVxzuLIQ.png)

### Step 6. Use VCS to manage your backup

As the linux user `jenkins` go to the directory `$JENKINS_HOME`, initiate a new git repository, and connect your local repository to remote VCS repository.

```bash
jenkins@host$ cd $JENKINS_HOME && git init
jenkins@host$ git remote add origin git@github.com:yu000hong/jenkins_backup

### Step 7. Test your Jenkins backup job

Click “Build now” button. If successful, you should see something like this:

![](https://miro.medium.com/max/1400/1*5HV6JLVZzNmZ_QNlyGKNZg.png)

## How to restore Jenkins from backup

### Step 1. Delete all from jenkins home directory

```bash
jenkins@host$ cd $JENKINS_HOME && rm -rf *
```

### Step 2. Pull data from your VCS repository

```bash
jenkins@host$ cd $JENKINS_HOME && git init
jenkins@host$ git clean -df # Cleans the working tree by recursively removing files that are not under version control
jenkins@host$ git remote add origin git@github.com:yu000hong/jenkins_backup
jenkins@host$ git pull origin master
```

### Step 3. Restart Jenkins daemon as root user

```bash
# service jenkins restart
```

Mission complete!

