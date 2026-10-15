# ansible

Ansible playbooks for provisioning and maintaining the nodes in a personal k3s cluster.

## Inventory

`inventory.ini` defines three groups:

- `k3s_control_node` — the k3s control plane node
- `k3s_worker_nodes` — the k3s worker nodes
- `everything_server` — a general-purpose host (also runs the NFS share used by the
  [`Kubernetes`](https://github.com/Kevin-Smith-112/Kubernetes) repo's PersistentVolumes)

## Playbooks

| Playbook | Purpose |
| --- | --- |
| `add_ssh_key.yml` | Installs the control host's public SSH key into `root`'s `authorized_keys` on every node |
| `nfs.yaml` | Installs and configures an NFS server (export directory, `/etc/exports`, firewalld rules) on `ansible.lily` |
| `update_controller.yml` | Runs a full `dnf update` on the k3s control node, one at a time (`serial: 1`) |
| `update_workers.yml` | Runs a full `dnf update` on the k3s worker nodes, one at a time (`serial: 1`) |

## Usage

```bash
ansible-playbook -i inventory.ini add_ssh_key.yml
ansible-playbook -i inventory.ini nfs.yaml
ansible-playbook -i inventory.ini update_controller.yml
ansible-playbook -i inventory.ini update_workers.yml
```

Nodes are reached over SSH as `root`; `inventory.ini` points at each host's key under
`~/.ssh/`. No credentials are stored in this repo.
