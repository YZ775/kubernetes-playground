VM_NAME := test-vm
VM_CPUS := 2
VM_MEM := 4G
VM_DISK := 32G

.PHONEY: launch
launch:
	multipass launch --name $(VM_NAME) --cpus $(VM_CPUS) --memory $(VM_MEM) --disk $(VM_DISK) --cloud-init cloud-init.yaml
