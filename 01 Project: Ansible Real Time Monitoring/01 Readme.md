# Ansible Real time Monitoring:

This project will help us to implement monitoring setup for organization using open source tools like Grafana, Prometheus, Alert Manager and other.

Steps:

1) Install `Prometheus` on Target system
2) Install `Grafana` on Target system
3) Install `Node Exporter` for exposing system data
4) Install `Alert Manager` for sending alert to Pagerduty/Slack
5) Create everything as a servers so starting stoping and handling will be easy

Create 2 AWS Linux EC2 instances(choose free tier for all and create a new pair to local PC)
![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/05b1d7e5-b06b-40e3-8d66-8653a5f1d4f1)

1) Ansible Server which has outbound to all
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/bd0eedb6-2d26-4f82-8a8d-9e48c482db19)

2) Target Node server which can intake the traffic from Ansible Server(1st instance); For now, Inbound can be ALL TCP in security groups.
   ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/6dc994c8-0e60-43aa-9172-7ab9f6e4b32d)

SSH Connection from One linux server to other server: 
Connecting to the Ansible instance(1st one):
- From Local, try to follow below steps
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/69db3328-afdd-47a4-9963-b552a9ed8760)
- After completion, It will look like:
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/013c9d61-49a6-43fe-84ef-8ebcae36d12e)

Log in as root: `sudo su -`
Installing Ansible: `sudo amazon-linux-extras install Ansible2` or `yum install ansible -y`
Checking Ansible: `ansible --version`
Renaming Ansible server name in CMD(Not mandatory step): `hostnamectl set-hostname Ansible_Server` -> This will replace ip address with Ansible_Server
Check SSH Keygen generation: 
- check if the `id_rsa.pub` is present in server or not: 
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/93cc2e91-996b-4cde-b05e-9caa688fa834)
- If not present like this case, we can use: `ssh-keygen` inside `/root/.ssh` directory.

 ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/99ac55fb-30b0-4728-814b-a128a2c1d3bb)

Now, we need to add this ssh keygen into the `authorized keys` in Target Node server location:
![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/328fd4ea-56e3-48cc-a2b7-d7b04758fd34)
Paste the ssh-keygen from 1st server to this: 
![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/c74ec35d-1656-4113-81c4-e9e2461ed8e4)

change the permissions of `authorized_keys` in Target Node server: `chmod 600 authorized_keys` -> 600 is read and write

Check connectivity between 2 servers(from 1st server): `ssh 18.234.82.145` -> public address of target server
![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/5f4da53b-a1f6-41c8-b4cd-fe2d6ef5ee38)

-----------------------

Creating Ansible Playbooks:


