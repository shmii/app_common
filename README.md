# Ansible Role: app-common
Ansible role to configure Applications Environement (disques, LVM, FS, Directories, users, etc ...)


This role :
  -  Configures disques, lvm, files systemes
  -  Create Applications Users
  -  Configure Applications directories


## Test and run environement


```
molecule 4.0.3 using python 3.10 
    ansible:2.10.8
    delegated:4.0.3 from molecule
    docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

```


## Requirements
## Requiered variables
This variables are mandatory for this role :
- application_name
- application_tag
- application_id
- application_user
<br>

## Role Variables
available variables are listed below, along with default values (see `defaults/main.yml`)
<br>

### `application_name`
**This var is mendatory !** <br>
Application Name and Description used for application local account.
```yaml
application_name: "My Application Name : And/or Short description"
``` 
<br>

### `application_tag`
**This var is mendatory !** <br>
This is the application's IT TAG. It's suppose to be a capital quadrigram.
```yaml
application_tag: "ITAG"
``` 
<br>

### `application_id`
**This var is mendatory !** <br>
 This is the application's IT TAG. It's supposed to be a tiny quadrigram equal to the `application_tag` but in tiny.
```yaml
application_id: "itag"
``` 
<br>

### `application_user`
**This var is mendatory !** <br>
 This is the application local user account login.
```yaml
application_user: "app_user_login"
``` 
<br>

### `application_group`
 This is the application local user account main group,  by default if not defined use same value as `application_user`
```yaml
application_group: "app_user_group"
``` 
<br>

### `application_groups`
 List of application local user account groups in extra to the main group,  by default no extra groups was set.
```yaml
application_groups: 
  - mygroupe
  - anothergroupe
  - appextragroupe
```

### `application_password`
 Application local user password, by default password is empty.
```yaml
application_password: "4pl1c4t10N_P455W0RD"
```

### `application_shell`
This is the application local user account default shell, by default user `/usr/sbin/nologin`
```yaml
application_password: "/usr/sbin/nologin"
```

### Application directories variables
Use `application_lvm_create` to chose if you wanted to create dedicate VGs and LVs for your application. This var is set to `false` by default. <br>
Use below vars to configure LVM, and there FilesSysteme
```yaml
application_directories:
  - Name: "Infra tools root Directory"
    path: /app/infra
    mode: '0755'
  - Name: "Test root Directory"
    path: /app/test
    mode: '0755'
```

### LVM variables (os_app_lvm... )
Use `application_lvm_create` to chose if you wanted to :
- Create dedicate VGs and LVs for your application.
- Create file systeme
- Creat and add Mount Point on fstab.
  
This var is set to `false` by default. <br>
If `application_lvm_create` set to true, mount point sould exist or set on `application_directories` list. <br>
Use below variables to configure les LVM, and their Filesystem.
```yaml
application_lvm_create: true 
application_lvm:
  - vg_name : "vg_app"
    vg_pvs :
    - pv_name : "lun1"
      pv_path: '/dev/disk/azure/scsi1/'
    vg_lvs :
    - lv_name: "lv_app"
      lv_size: '10g'
      fs: 'xfs'
      mount_point: /app/{{ application_tag }}/app
    - lv_name: "lv_log"
      lv_size: '10g'
      fs: 'xfs'
      mount_point: /app/{{ application_tag }}/log
    - lv_name: "lv_data"
      lv_size: '10g'
      fs: 'xfs'
      mount_point: /app/{{ application_tag }}/data
```

## Dependencies

None.

## Example Playbook

```yaml
    - hosts: server
      roles:
        - { role: shmii.app-common }
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
