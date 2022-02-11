Make sure to uninstall ansible from env_fun


ansible             ansible-connection  ansible-doc         ansible-inventory   ansible-pull        ansible-vault       
ansible-config      ansible-console     ansible-galaxy      ansible-playbook    ansible-test


Try using localhost SSH servers as managed nodes
Try using Ubuntu docker images with SSH installed as managed nodes



git clone https://github.com/art267/docker-ubuntu-sshd/
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' jovial_khorana



ansible --inventory inventory.txt --user master --ask-pass all -m ping
ansible --inventory inventory.txt --user master --ask-pass all -a "/bin/echo hello"



# create mytask.yaml
ansible-playbook --inventory inventory.txt --user master --ask-pass mytask.yaml
ssh 172.17.0.2
$ ls /tmp     # It should be there
