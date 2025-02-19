# Ansible roles

Nginx role is created using :
~~~
$ ansible-galaxy init Nginx
~~~

Copy Nginx folder into /etc/ansible/roles/

Then run the playbook to trigger Nginx role :
~~~
$ ansible-playbook nginx_via_role.yml
~~~
