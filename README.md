# Ansible Role: os-common
Ansible role to configure Linux OS (Security, prerequisite packages and tools, etc ...)


This role :
  -  Install and configures Essentiels Extra Repositories (epel, powertools)
  -  Install useful packages or sysadmin tools
  -  Create base environement for systeme and infra administrators (users, directories, etc ...)
  -  Create base environement for applications and servicies


## Test and run environement


```
molecule 4.0.3 using python 3.10 
    ansible:2.10.8
    delegated:4.0.3 from molecule
    docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

```


## Requirements

None.

## Role Variables

vailable variables are listed below, along with default values (see `defaults/main.yml`):

### os_app_directories

Use `os_app_directories` to define needed applications direcories

```yaml
os_app_directories:
  - Name: "Application root directory"
    path: /app
    mode: '0755'
```

### os_infra_directories

Use `os_infra_directories` to define needed applications direcories

```yaml
os_infra_directories:
  - Name: "Infra tools root Directory"
    path: /app/infra
    mode: '0755'
  - Name: "Test root Directory"
    path: /app/test
    mode: '0755'
```

## Dependencies

None.

## Example Playbook

```yaml
    - hosts: server
      roles:
        - { role: shmii.os-common }
```

## Warning / known bugs

/!\ To validate this role with "molecule" from Ubuntu @ WSL/WSL2 it is necessary to create the folder  `/sys/fs/cgroup/systemd` on the host linux subsystem. 


`sudo mkdir /sys/fs/cgroup/systemd`

This directory is necessary for some os (RHEL9, Rocky9, etc...) and not existing on local Ubuntu WSL sub Systeme. 

## Sources and Bibliography


Redde Caesari quae sunt Caesaris, et quae sunt Dei Deo !
 
- Jeff GEERLING ( https://www.linkedin.com/in/jeff-geerling-086bb2a/  --- https://github.com/geerlingguy)
- Stephane ROBERT ( https://www.linkedin.com/in/stephanerobert1/  ---  https://blog.stephane-robert.info)



## License

GNU GENERAL PUBLIC LICENSE Version 3, 29 June 2007

## Author Information

This role was created in 2022 by [Thomas CHALMEL] (https://www.thomas.chalmel.org/).
