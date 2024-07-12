# devops-case-study
DevOps Case Study

Ansible stages
-----------
'''
"ansible all --list-hosts -v"
'''
## First of all,create a logrotate folder 
'''
mkdir /etc/cron.d/logrotate_cron
'''
'*/10 2-3 * * * root /usr/sbin/logrotate /etc/logrotate.conf'
then create yml file 
logrotate_cron.yml
'
---
- name: Deploy logrotate cron job
  hosts: all
  become: yes
  tasks:
    - name: Ensure logrotate cron job is present
      cron:
        name: "logrotate"
        minute: "*/10"
        hour: "2-3"
        user: "root"
        job: "/usr/sbin/logrotate /etc/logrotate.conf"
		'
#  Run the command of playbook 
'ansible-playbook logrotate_cron.yml'

Docker/Kubernetes
---------------
# Create a network bridge :  
'	docker network create --subnet=172.20.8.0/24 custom_bridge_network'
#Create a docker-compose.yml file 
'
version: '3.8'

services:
  nginx:
    image: nginx:latest
    container_name: nginx_server
    networks:
      custom_bridge_network:
        ipv4_address: 172.20.8.2
    volumes:

      - ./nginx_logs:/var/log/nginx
    ports:
      - "80:80"

networks:
  custom_bridge_network:
    external: true
'

# Create the directory for Nginx log
'mkdir -p ./nginx_logs
'
# DOcker compose up 
' docker-compose up -d'
2)	Which Kubernetes command you will use to identify the reason for a pod restart in the project "internal" under namespace "production".
Answer:
'kubectl describe pod my-pod -n production'

MAVEN_APPLICATION_INITIALIZATION
--------------------------------
mvn archetype:generate -DgroupId=com.tcs -DartifactId=DevOpsCaseStudy -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false  

RUN_JENKINS
-----------
docker run -u root --name jenkins --rm -d -p 80:8080 -p 50000:50000 -v /root/workspace/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins

RUN_SONARQUBE_SERVER
--------------------
docker run -d --rm --name sonarqube -p 9000:9000 sonarqube

SLAVE_INSTALLATIONS
====================
INSTALL_JAVA
-------------------
sudo apt-get update
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get install openjdk-8-jdk

INSTALL MAVEN
-------------------
sudo apt install maven

INSTALL_CHROMIUM_BROWSER
-------------------
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:canonical-chromium-builds/stage
sudo apt-get update
sudo apt-get install chromium-browser

INSTALL_XVFB
-------------------
sudo apt install xvfb
Xvfb :0 >& /dev/null &

RUN_APPLICATION
-------------------
docker run -itd --name app --rm -p 8080:8080 rmani1/app:v1

GCLOUD_CONFIGURATION
-------------------
gcloud config configurations list
gcloud config configurations activate CONFIGURATION_NAME
gcloud config set compute/zone ZONE
gcloud config set compute/region REGION
export CLOUDSDK_COMPUTE_ZONE=asia-south1-c
export CLOUDSDK_COMPUTE_REGION=asia-south1

CREATE_CLUSTER
-------------------
gcloud container clusters create devops-case-study \
--num-nodes 2 \
--machine-type n1-standard-2 \
--scopes "https://www.googleapis.com/auth/projecthosting,cloud-platform" \
--zone asia-south1-c

CLUSTER_SERVICE_ACCOUNTS
-------------------
kubectl get serviceAccounts
creat service ServiceAccount
kubectl get serviceaccounts/build-robot -o yaml
kubectl -n serviceids get secret $(kubectl -n serviceids get secret | grep build-robot | awk '{print $1}') -o json | jq -r '.data.token'  | base64 -d
TOKEN=`kubectl get secret $(kubectl get secret | grep build-robot | awk '{print $1}') -o json | jq -r '.data.token' | base64 -d`
kubectl get deployments --server=https://35.244.45.23 --token=$TOKEN
kubectl get services lb-devops-case-study -o=jsonpath='{.status.loadBalancer.ingress[0].ip}'

INSTALL_MONITORING_AGENT
-------------------
curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
sudo bash install-monitoring-agent.sh
