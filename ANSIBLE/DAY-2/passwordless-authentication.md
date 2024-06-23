using public key

1) create the public in controller node using this command
    ssh-keygen -t rsa -b 2048

2) create ansible configuration file
    mkdir -p /etc/ansible
    vim ansible.cfg
        [defaults]
        inventory = /etc/ansible/hosts
        Host key checking = No
3) check the config file set properly or not if config is set properly we will be able to see "config file = /etc/ansible/ansible.cfg"

    ansible --version
    ansible [core 2.16.3]
  config file = /etc/ansible/ansible.cfg

4) update the hosts file (/etc/ansible/hosts) is the file in my case

    <public-ip> ansible_user=root ansible_password=root ansible_connection=ssh ansible_become=yes

5) copy the public key from controller to managed node under .ssh/authorizedkeys and give 644 permissions to the file

6) under this /etc/ssh/sshd_config made some changes in that file

    Port 22

    PermitRootLogin yes

    PasswordAuthentication no

7) ansible -m ping all
44.204.146.133 | FAILED! => {
    "msg": "to use the 'ssh' connection type with passwords or pkcs11_provider, you must install the sshpass program"
}

8) if we get this type of error install this package and try again, then we will able to see the suceess connection

    sudo apt update
    sudo apt install sshpass

9) ansible -m ping all
44.204.146.133 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}