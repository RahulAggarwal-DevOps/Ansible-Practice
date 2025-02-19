# Ansible
Ansible is an (IaC) automation tool that helps with configuration management, application deployment, and more. It's used by IT professionals to automate repetitive tasks and workflows.

Now that the infra is ready, I also want to configure the infra using code. Meaning - I created multiple AWS EC2 instances using terraform, but I also want to run commands/scripts in all those instances in one go, via a code. That's what ANSIBLE is doing. I just need to let ansible know about the hosts to operate with, and write some YMLs to configure those hosts, and BOOM.....all sorted.
Ansible uses SSH to communicate with the hosts. 

Ansible has a default inventory located at /etc/ansible/hosts, where we can create group of hosts to work with (IP addresses), set variables to communicate with those hosts, like username, private keys, python interpreter, and done!!!
We can have our self-defined inventories as well.
~~~
[UbuntuServers] # group name
server1 ansible_host=IP
server2 ansible_host=IP

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=<path-to-private-key>
~~~
You can list down the inventories as below:
~~~
$ ansible-inventory --list
or
$ ansible-inventory --list -i <path to your inventory>
~~~

Now, to verify if Ansible can communicate with the hosts in this inventory (where group name is UbuntuServers) using ping module :
~~~
$ ansible -m ping UbuntuServers
~~~

We can run ad-hoc commands on our hosts:
~~~
$ ansible -a "free -h" UbuntuServers
~~~

However, to run a series of commands in an arranged format on our hosts, we can use playbooks. Playbook is a YML file, that defines tasks/commands to run on the hosts. Kindly refer my playbooks in playbooks folder above.
To run a playbook :
~~~
$ ansible-playbook <playbook_file.yml>
~~~

Ansible Galaxy is very similar to Helm Charts we use in Kubernetes. Galaxy contains roles to configure any infrastructure. Roles are nothing but a folder that contain files to conveniently arrange our playbook's content. For example, if you want to configure your hosts with Nginx, you can use Nginx role available in the Ansible Galaxy Collection as below:
~~~
$ ansible-galaxy collection install nginxinc.nginx_core
~~~
You will find Nginx role installed at /etc/ansible/roles/. 

But the fun lies in creating our own roles, right??? Simply initialize a folder with Ansible Galaxy, ansible will create file structure (inside role) that will ease you in writing your playbook components in a more managed form. Run the below command to initialize Nginx folder with Ansible Galaxy:
~~~
$ ansible-galaxy init Nginx
~~~
This will create Nginx folder, whose tree structure is as below:
~~~
../Roles/NGINX/Nginx/
├── README.md
├── defaults
│  └── main.yml
├── files
│  └── index.html
├── handlers
│  └── main.yml
├── meta
│  └── main.yml
├── tasks
│  └── main.yml
├── templates
├── tests
│  ├── inventory
│  └── test.yml
└── vars
 └── main.yml
~~~
Just add author information in the meta/main.yml, all your tasks in the tasks/main.yml, variables used in the tasks in vars/main.yml, put files (if used in the tasks) in /files/. and done!!!

Just note that this Nginx role (folder) must be moved to /etc/ansible/roles/ to work. And then, create a YML to trigger this role :
~~~
-

 name: Install Nginx and Deploy a Webpage
 hosts: UbuntuServers
 become: yes
 roles:
 - Nginx
~~~
~~~
$ ansible-playbook <file.yml>
~~~
One similar role is available in the Roles/NGINX folder.
