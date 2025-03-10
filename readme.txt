How to Run the Playbook

    Ensure Ansible is installed on your machine.
    Define your hosts in inventory.ini:

[webservers]
your_server_ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

Run the playbook:

ansible-playbook -i inventory.ini wordpress-lemp.yml

Your public key (id_rsa.pub) must be added to /home/ubuntu/.ssh/authorized_keys on the remote server.
