# Ansible Inventory

The default ansible inventory is at /etc/ansible/hosts

However, we can create our own inventories as seen above.

Now, to call hosts mentioned in these user-defined inventories, use (-i) flag, as below:
~~~
$ ansible-inventory --list -i development
$ ansible-inventory --list -i production
$ ansible -m ping -i development
$ ansible -a "free -h" -i production
$ ansible-playbook <filename.yml> -i inventories
~~~
