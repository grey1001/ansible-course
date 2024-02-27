ansible-galaxy install --roles-path . -r requirements.yaml # Used to install roles in the specified path using the requirements.yaml file
ansible-galaxy init # used to create your role
Dependencies run first when specified in meta/main.yaml
ansible-playbook -i "inventoryfile" "path to playbook"
ansible all -m ping -i "inventoryfile"
export ANSIBLE_HOST_KEY_CHECKING=False  # to diable hostkey checking
ansible-inventory -i demo.aws_ec2.yaml --graph # to list all your hosts
using the "serial" option when using dynamic inventory allows to stagger the configuration of instances at a time
use "vars"in your playbook configuiration to pass connection information when using dynamic inventory
we can also use  percentages with "serial"
use lookup module when integrating vault with ansible
to get your path when using vault check the path or use the cli command when dealing with kv 