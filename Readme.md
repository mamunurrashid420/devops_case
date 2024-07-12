# devops-case-study
DevOps Case Study


Ansible stages
-----------
```
"ansible all --list-hosts -v"
```
### First of all,create a logrotate folder 
```
mkdir /etc/cron.d/logrotate_cron

'*/10 2-3 * * * root /usr/sbin/logrotate /etc/logrotate.conf'
```
then create yml file 
logrotate_cron.yml
'
```
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
```
#  Run the command of playbook 
```
ansible-playbook logrotate_cron.yml
```

Docker/Kubernetes
---------------
# Create a network bridge :  
```
docker network create --subnet=172.20.8.0/24 custom_bridge_network

```
#Create a docker-compose.yml file 
```
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
```

# Create the directory for Nginx log
```
mkdir -p ./nginx_logs
```
# DOcker compose up 
```
 docker-compose up -d
 ```
2)	Which Kubernetes command you will use to identify the reason for a pod restart in the project "internal" under namespace "production".
```
 kubectl describe pod my-pod -n production
 ```
