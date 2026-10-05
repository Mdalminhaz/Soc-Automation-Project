# Soc-Automation-Project
Automating SOC using Wazuh, TheHive, and Shuffle 


## Overview

This project is going to show you how i automate soc operations using wazuh and TheHive while utilizing shuffle.

### Diagram Overview

<img width="1140" height="860" alt="Soc Automation rundown" src="https://github.com/user-attachments/assets/346894c4-14ed-4c41-b1cf-b10be6af90a1" />

This shows the workflow step by step and how we plan to tackle this project.

#### Step 1: Choose your Server
i used Vultr to tackle the server portion (you can use Azure or AWS to host the server

I opened two VMs one hosting TheHive and the other for Wazuh 

<img width="1490" height="436" alt="image" src="https://github.com/user-attachments/assets/22dd9106-49bd-4f58-b81c-1cd44c338f42" />


#### Step 2: Access Wazuh Manager

* firstly SSH into your wazuh via the console on vultr or your personal CMD

with the command "ssh username@yourpublicip" followed by your password


* after you're signed in you can access your wazuh Manager using your username and password which can be found by following the wazuh installation module.

<img width="1881" height="895" alt="image" src="https://github.com/user-attachments/assets/2d6af689-6dbf-4063-b5c5-bba0179dafd2" />

#### Step 3: Access and configure Cassandra

* firstly access TheHive using ssh again using your personal CMD

with the command ssh username@yourpublic ip followed by your password

* after we're gonna want to configure the Cassandra storage file

 by using the command: nano etc/cassandra/cassandra.yml

Change the following:

###### (Small tip Use Cntrl w to search through what you need)

* cluster_name: whatever name you want
* listen_address: TheHive instance IP address
* rpc_address: TheHive instance IP Address 

then after press cntrl x then y to save then enter

following this we have to stop the process by stopping Cassandra

###### (small tip using tab helps auto complete commands)

using the command: systemctl stop cassandra.service

rm -rf /var/lib/cassandra/*

then we will start the new Cassandra with our new configurations 

using command: systemctl start cassandra.service 
followed by systemctl status cassandra.service

<img width="887" height="62" alt="image" src="https://github.com/user-attachments/assets/f80801e0-c064-474e-9c6c-287a75808d12" />

#### Step 3a: Configure Elastic search

we first  start by

Entering the following command: nano /etc/elasticsearch/elasticsearch.yml

change the following:
* change cluster.name: to whatevername you want and remove the # 
* change node.name: remove the #
* change network.host: the public ip of ur instance and remove #
* change http.port:9200 by removing #
* change cluster.initial_master_nodes: get rid of node 2 and the #

save it by using: Cntl + x then press y then Enter

type in the following to start: Systemctl start elasticsearch 
then type: systemctl status elasticsearch (to make sure its goood)

<img width="1232" height="66" alt="image" src="https://github.com/user-attachments/assets/17836af7-cb8a-41bb-ba90-efdeb2f84f66" />


#### Step 4: Configure the Hive

before we start we want to make sure we change owner from root so that the owner and group is TheHive

we first check by cd to the directory with the files 

By typing the command: /opt/thp# ll (ll shows the files and permissions) 
then to change the permissions: chown -R thehive:thehive /opt/thp

Should look like this:

<img width="737" height="295" alt="image" src="https://github.com/user-attachments/assets/402098d7-380b-4ba1-bfda-308c2ef6b3cc" />

Then to configure TheHuve
type: nano /etc/thehive/application.conf 

change the following: 
* change hostname to your public ip
* change clustername to the same one as before
* change hostname under elasticsearch to your public ip
* then change application baseURL localhost to your public ip address

Then you can save by cntrl x then y then enter

You can now start TheHive

type the following commands: 
Systemctl start thehive
Systemctl enable thehive
systemctl status thehive

<img width="1047" height="85" alt="image" src="https://github.com/user-attachments/assets/c93c55d2-b1b5-48ea-8857-7667a5f362be" />


#### Quick Check 

Before we continue lets make sure everything is up and running

type the following:

systemctl status cassandra.service
<img width="877" height="70" alt="image" src="https://github.com/user-attachments/assets/7fac999f-d184-488c-8af8-706a0c138ee0" />


systemctl status elasticsearch.service
<img width="1181" height="65" alt="image" src="https://github.com/user-attachments/assets/8ff37240-cffd-467e-85af-bc17ad3fbe36" />

systemctl status thehvie.service
<img width="1046" height="67" alt="image" src="https://github.com/user-attachments/assets/48aab1e2-e495-46c2-a360-17d0bbcfb8ac" />

#### Step 5: access your Hive Webpage




