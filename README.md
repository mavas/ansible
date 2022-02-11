# Ansible work

## Todo items
- Make sure to uninstall ansible from env_fun
- Try using localhost SSH servers as managed nodes
- Try using Ubuntu docker images with SSH installed as managed nodes

## Some commands
When I tab-complete `ansible` after first time installing it in virtual_env:

```
ansible             ansible-connection  ansible-doc         ansible-inventory   ansible-pull        ansible-vault       
ansible-config      ansible-console     ansible-galaxy      ansible-playbook    ansible-test
```

## An Ubuntu Docker image with SSHD started/included.
```
git clone https://github.com/art267/docker-ubuntu-sshd/
```
The strategy is to use the host OS as the controller and one or more docker Ubuntu containers as managed nodes.

## Example commands
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
