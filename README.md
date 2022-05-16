# Update

## Variables

```yaml
# wether to reboot the node after an update was performed
update_reboot: bool | default(true)

# delay before rebooting the node
# useful to delay the reboot of non-redundant network nodes that provide the connection to another node in the play
# also useful to stagger the reboot of redundant nodes
update_pre_reboot_delay: int | default(5)

# delay before trying to confirm the reboot was successfull
# useful for nodes knows to have longer boot times
update_post_reboot_delay: int | default(5)

# timeout after which a node is considered to have failed to reboot
update_reboot_timeout: int | default(600)
```

## Expample Usage

### Inventory

```yaml
---
all:
  vars:
    ansible_connection: ssh
    ansible_python_interpreter: /usr/bin/python
    ansible_ssh_user: admin
    update_reboot_timeout: 120
  children:
    firewall:
      vars:
        update_pre_reboot_delay: 150
      hosts:
        fw-01:
    reverse_proxy:
      hosts:
        cache-01:
    git:
      hosts:
        git-01:
    pad:
      hosts:
        pad-01:    
```

### Playbook

```yaml
---
- name: update all nodes
  hosts: [all]
  become: yes
  become_user: root
  roles:
  - update
```
