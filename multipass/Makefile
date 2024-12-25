VM_NAME1 := k8s-1
VM_NAME2 := k8s-2
VM_NAME3 := k8s-3
VM_CPUS := 2
VM_MEM := 4G
VM_DISK := 32G

.PHONEY: launch
launch:
	multipass launch --name $(VM_NAME1) --cpus $(VM_CPUS) --memory $(VM_MEM) --disk $(VM_DISK) --cloud-init cloud-init.yaml
	multipass launch --name $(VM_NAME2) --cpus $(VM_CPUS) --memory $(VM_MEM) --disk $(VM_DISK) --cloud-init cloud-init.yaml
	multipass launch --name $(VM_NAME3) --cpus $(VM_CPUS) --memory $(VM_MEM) --disk $(VM_DISK) --cloud-init cloud-init.yaml

.PHONEY: set-node-ip
set-node-ip:
	$(eval NODE_IP1 := $(shell multipass info k8s-1 | grep IPv4 | awk '{print $$2}'))
	$(eval NODE_IP2 := $(shell multipass info k8s-2 | grep IPv4 | awk '{print $$2}'))
	$(eval NODE_IP3 := $(shell multipass info k8s-3 | grep IPv4 | awk '{print $$2}'))
	yq -i '.k8sNodes.hosts.k8s-1.ansible_host="$(NODE_IP1)"' ../ansible/inventory.yaml
	yq -i '.k8sNodes.hosts.k8s-2.ansible_host="$(NODE_IP2)"' ../ansible/inventory.yaml
	yq -i '.k8sNodes.hosts.k8s-3.ansible_host="$(NODE_IP3)"' ../ansible/inventory.yaml
	yq -i '.[].vars.node1="$(NODE_IP1)"' ../ansible/playbook.yaml
	yq -i '.[].vars.node2="$(NODE_IP2)"' ../ansible/playbook.yaml
	yq -i '.[].vars.node3="$(NODE_IP3)"' ../ansible/playbook.yaml


