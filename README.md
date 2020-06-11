# Edureka Project Certificate for DevOps submitted by Abhishek Chaudhry

## Batch: 20 April - 13 May, 2020

## Link: https://github.com/xsabhishek/projCert

###  Checklist for Jobs

1. Install and configure puppet agent on the slave node (Job 1) 

2. Sign the puppet certificate on master using Jenkins (Job 2)

3. Trigger the puppet agent on test server to install docker (Job 3)

4. Pull the PHP website, Dockerfile and Selenium JAR from your git repo and build and deploy
your PHP docker container. After this test the deployment using Selenium JAR file. (Job 4)

5. If Job 4 fails, delete the running container on Test Server

### Steps for executing the project

1. Setup connection on Jenkins with Master node and Slave Node. It includes creating a slave node through master node Jenkins Dashboard.
2. Setup Puppet on Slave node. The host files need to be updated with the master and slave IPs. Connect Puppet with Master VM. 
3. Sign the certificate of Slave VM on Master VM
```
puppet cert list --all
puppet cert sign --all
```
4. Create a puppet file to install docker with content as the one in the repository (init.pp). 
```
pdk new module install-docker
cd install-docker/manifests 
vim init.pp
pdk build  install-docker
opt/puppetlabs/bin/puppet module install /home/edureka/modules/install-docker/pkg/filename.tar.gz
```
5. In the slave VM, execute  
`/opt/puppetlabs/bin/puppet agent -t `
6. Ansible playbooks need to be executed on with the Master VM. The hosts file needs to be appended with the IP of the host VM. The contents of the file are in chrome_playbook.yml and git_playbook.yml.
```
ansible-playbook chrome_playbook.yml
ansible-playbook git_playbook.yml
``` 
5. Jenkins Pipeline needs to be created with the jobs interconnected. The pipeline will execute on Slave VM. The first job of Jenkins instructs to pull the repository and build the DockerFile. So, the Github link is provided along with credentials in SCM section. And in build section, the Dockerfile is built. 
6. Second Job instructs to run the test.jar file. It should give a Fail result when run. The job has been setup such that when it fails it will move on to execute third job. 
7. As the Second job failed, the third job will run which is instructed to stop the running container. The docker plugin in Jenkins allows to start/stop the running container. Afer stopping, the container is deleted.   
