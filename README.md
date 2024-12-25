# Ansible playbook to setup Kubernetes cluster by using kubeadm

## Prerequisites

- yq
- multipass
- ansible

## Procedure

1. Execute ansible playbook to setup kubeadm

```bash
cd multipass
yq -i '.users.[0].ssh_import_id="your_github_id"' multipass/cloud-init.yaml
make launch
make set-node-ip
cd ../ansible
ansible-playbook -i inventory.yaml playbook.yaml
```

2. Initialize the first node as control-plane

```bash
# @ one node
sudo kubeadm init --control-plane-endpoint 127.0.0.1:5443 --upload-certs
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

3. Join another nodes as control-plane

```bash
# @ anothor node
# follow the instruction from the output of kubeadm init at the first node
kubeadm join 127.0.0.1:5443 --token s6v38r.omcwhyjjckkg46sq \
        --discovery-token-ca-cert-hash sha256:19b73781cf9474750984f508e0969e4d359dbf8e7bb386a094a6d37130caa669 \
        --control-plane --certificate-key c12847e26dc828f04bdd41d9be71893d9e3209816ac3cc90d8a33401054acef3
```

4. Install CNI

TBD
