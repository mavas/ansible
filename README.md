# Ansible work

Some beginner's work I've done with learning Ansible.

## Planning/features

### Probably not now

The following isn't probably needed because it's not Ansible itself per-se: it's just *cool/interesting* (so do it later).

- Maybe use docker-compose.yaml to deal with more than one container
- Maybe use Ansible + Docker modules [1](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html) [2](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html) to do things on hosts.

## An Ubuntu Docker image with SSHD started/included.

This repository was perfect for this work, as it is just the usual Ubuntu image, but with everything else needed to also make sure that an SSH server was start on port 22:
```
git clone https://github.com/art267/docker-ubuntu-sshd/
cd docker-ubuntu-sshd
docker build -t art567/ubuntu .
docker run --interactive --tty --rm --publish 22 art567/ubuntu
```
The strategy is to use the host OS as the controller and one or more docker Ubuntu containers as managed nodes.  Currently, there is just one Ubuntu container running SSH, but maybe docker-compose will be used to experiment with Ansible modifying multiple managed nodes later.

## Some commands
When I tab-complete `ansible` after first time installing it in virtual_env:

```
ansible             ansible-connection  ansible-doc         ansible-inventory   ansible-pull        ansible-vault
ansible-config      ansible-console     ansible-galaxy      ansible-playbook    ansible-test
```

## Things learned; example commands
Tells you the IP address of a Docker image:
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' jovial_khorana
```

These commands worked:
```
ansible --inventory inventory.txt --user master --ask-pass all -m ping
ansible --inventory inventory.txt --user master --ask-pass all -a "/bin/echo hello"
```

Trying out the playbooks:
```
ansible-playbook --inventory inventory.txt --user master --ask-pass mytask.yaml
ssh 172.17.0.2
$ ls /tmp               # It should be there
```

Running a command via the shell at the node.  This is different from the `-a` option example from above:
```
user@machine:~/ansible$ ansible --inventory inventory.txt --user master --ask-pass all -m ansible.builtin.shell -a 'ls /tmp'
SSH password: 
172.17.0.2 | CHANGED | rc=0 >>
ansible_ansible.legacy.command_payload_wl40k0fz
ansible_was_here
```

Started dealing with playbooks:
```
ansible-playbook --inventory inventory.txt --ask-pass --check playbook.yml
```

Create playbook to create a file, verify it's there with output, and then remove the file, and verify it's removed:
```
ansible-playbook --inventory inventory.txt --user master --ask-pass playbook.yml
docker exec --interactive --tty keen_haibt ls /tmp
```


ansible --inventory inventory.txt --user master --ask-pass all -m ansible.builtin.stat -a "path=/tmp/love.txt"
SSH password:
172.17.0.2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "stat": {
        "exists": false
    }
}
