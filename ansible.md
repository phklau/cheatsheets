# Ansible

## Playbook

Playbooks defines a lot of small taks (so called plays, e.g. change one line in
a config file). These plays could be grouped in tasks (e.g. make ssh config
changes). Those tasks composed are a role (e.g. manage a webserver).
All those roles can be called in a playbook file and played on different hosts.

Project-structure:
```
Playbook
├── ansible.cfg
├── group_vars
│   └── all
│       └── secrets
├── hosts
├── roles
│   └── my_role
│       ├── handlers
│       │   └── main.yml
│       └── tasks
│           ├── main.yml
│           └── my_task.yml
└── run.yml
```

```
run.yml
- name: Description
  hosts: webservers
  become: yes
  roles:
    - role: install_appache
    - role: deploy_app
- name: Description
  hosts: sqlservers
  become: yes
  roles:
    - role: install_maria_db
    - role: setup_db
- name: Description
  hosts: all
  become: yes
  roles:
    - update
    - backup
```

### Hosts

Define different inventorys
```
all:
    hosts:
        my_host:
            ansible_host: "192.168.X.X"
            ansible_user: user
            ansible_ssh_private_key_file: 
            my_host_var: lala
    vars:
        ansible_connection: ssh
        ansible_become_user: root
        ansible_become_method: su
        ansible_become_password: "{{ root_pw }}"
```
### Roles

`main.yml` defines all tasks of this role. Could also be included:
```
main.yml:
- include_tasks: TASK_NAME.yml
- include_tasks: TASK_NAME.yml
```

### Tasks


```
- name: Description here
    MODULE_NAME:
        MODULE_PARAMS:
```
### Variables

Defined varibles in these files will be loaded (usefull for vault files):
- group_vars (defined per group)
```
group_vars
└── all
    └── secrets
└── my_group
    └── my_vars
```

- host_vars (defined per host)

```
host_vars
└── all
    └── secrets
└── my_host
    └── my_vars
```

### Handlers

Handlers will be triggered only on change. E.g. restarting sshd-service after
config change. Use notify or listen to send/connect the signal.

```
handlers/main.yml
- name: my_handler
  service:
    name: sshd
    state: restarted
#   listen: TASK_THAT_TRIGGERS_HANDLER
role/some_role/tasks/some_task.yml
- name: this task triggers my_handler
    module:
    notify: my_handler
```

## CLI

```
ansible-playbook run.yaml \
	--ask-vault-pass \ 
	-K # ask become password (eg sudo)
```

