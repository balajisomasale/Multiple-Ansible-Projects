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

## SSH Connection from One linux server to other server: 
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

![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/6dadfd3b-a963-442c-bf3b-a11cc18490d9)


change the permissions of `authorized_keys` in Target Node server: `chmod 600 authorized_keys` -> 600 is read and write

Check connectivity between 2 servers(from 1st server): `ssh 18.234.82.145` -> public address of target server
![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/5f4da53b-a1f6-41c8-b4cd-fe2d6ef5ee38)

-----------------------

Creating Ansible Playbooks:

- Download the playbooks from this Github repo: `git clone https://github.com/balajisomasale/Multiple-Ansible-Projects.git`
- Go to current project and change the inventory file with public IP address of Target Node Server: 

  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/2168ac3b-f7c7-456a-b16d-2d5df3ecc9c9)

- Run the ansible playbook using command: `ansible-playbook -i inventory playbook.yml`

- Navigate to target server to check the changes: 
  - To check the grafana status: `systemctl status grafana-server`
    ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/93d41f08-6f43-4497-9d6b-1eec4512fbd7)
  - To check the Prometheus status: `systemctl status prometheus`
    ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/5afd4100-f89f-4ecc-941b-81fa9d1313a1)
  - To check the Alert Manager status: `systemctl status alertmanager`
    ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/0fdbbbc0-13e2-4e0e-bff1-718b6c1882b5)
  - To check the Node Exporter status: `systemctl status node_exporter.service`
    ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/2ff1bb0f-af57-406f-b3b0-35810f145866)

Validation of the servers in the browser:
- Checking Grafana in browser with `3000` as port with userid and pwd as `admin`:
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/0dee5e92-18db-4f55-b32f-6025d2e6b726)
- Checking prometheus port `9090`:
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/da1152ba-afd8-4286-a4aa-cd063b513d2f)
- Checking Node exporter port `9100 `:
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/c1094086-e54a-4891-9143-1025d44aa739)
- Checking Alert Manager port `9093`:
  ![image](https://github.com/balajisomasale/Multiple-Ansible-Projects/assets/35003840/b5ee9730-9cf4-4699-9360-1093943d3588)

By this, we can monitor the logs real time with the usage of Ansible playbooks
---------------- EOF----
