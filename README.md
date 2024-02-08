# Ansible Role: app-common
This Ansible role provided the Application base (disques, LVM, FS, Directories, users, etc ...)


This role :
  -  Configures Applications disques, lvms, files systemes
  -  Create Applications groups, Users, etc ...
  -  Configure Applications directories


## Test and run environement


```console
$ molecule --version
molecule <green>4.0.3</green> using python 3.10 
    ansible:2.13.5
    delegated:4.0.3 from molecule
    docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

```



## Requirements

<br>

> ### Requiered Variables
> <BR>
>
> ### Role Variables
> ---
> Available variables are listed below, along with default values (see `defaults/main.yml`)<br>
> <br>
> 
> > #### The `application` Dictionary
> > ---
> > This dictionary gathers all **Application** variables.<br>
> > It should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory !** <br> 
> > **Warning ! Only One application must be set on this dictonary**
> > *( Future update should improve that ! )* <br>
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >     application:
> >       - name: "My Test Application"
> >         tag: APTEST
> >         id: aptest
> >        comment: "My TEST Application"
> > ``` 
> >
> > ##### `application.name`  **This var is mendatory !**
> > The application Name. <br>
> > 
> > ##### `application.tag` **This var is mendatory !**
> > This is the application's TAG.<br>
> > It's suppose to be a capital quadrigram on my organisation.<br>
> >  *But your free to use what you wanted !*<br>
> >
> > ##### `application.id` **This var is mendatory !**
> > This is the application's id. <br>
> > It's supposed to be a tiny quadrigram equal to the `application.tag` but in tiny. <br>
> >
> > ##### `application.comment` **This var is mendatory !**
> > The application Name and Description<br>
> > <br>
> >
> >
> > ##### `application_groups_create` **This var is mendatory !**
> > This bolean variable used to enable or disable groups creation.<br>
> > This variable should be declare on inventory `group_vars`.<br>
> > **This variable is mendatory !** <br> 
> > **If this variable is set to `true` you must set the `application_groups` Dictionary !**<br>
> > <br>
> >
> >
> > > #### The `application_groups` dictionaries.
> > ---
> > This dictionary is used, ang group created only if the `application_groups_create` var is set to true.
> > This dictionary gathers all Application **Groups** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory if `application_groups_create` is set to true !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >     application_groups:
> >       - name: groupe_one
> >       - name: groupe_two
> >       - name: groupe_three
> > ``` 
> > ##### `application_groups.name`  **This var is mendatory !**
> > The groups name. <br>
> > <br>
> > 
> > 
> > #### The `application_users` dictionaries.
> > ---
> > This dictionary gathers all Application **Users** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >     application_users:
> >      - login: "user_one"
> >        comment: "This is User One account"
> >        home_directory: "/app/user_one"
> >        shell: "/usr/sbin/nologin"
> >        group: "user_one"
> >        groups:
> >          - groupe_one
> >          - groupe_two
> >      - login: "user_two"
> >        comment: "This is User Two account"
> >        home_directory: "/app/user_two"
> >        shell: "/usr/sbin/nologin"
> >        group: "user_two"
> >        groups:
> >          - groupe_three
> > ``` 
> > ##### `application_users.login`  **This var is mendatory !**
> > The user's login. <br>
> > 
> > ##### `application_users.comment` **This var is mendatory !**
> > The user's description.<br>
> >
> > ##### `application_users.home_directory` **This var is mendatory !**
> > The user's home directory. <br>
> >
> > ##### `application_users.shell` **This var is mendatory !**
> > The user's default shell<br>
> >
> > ##### `application_users.group` **This var is mendatory !**
> > The user's default group<br>
> >
> > ##### `application_users.groups` **This var is mendatory !**
> > The user's extra groups<br>
> > <br>
> >
> >
> > #### The `application_directories` Dictionary
> > ---
> > This dictionary gathers all Application **Directories** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >    application_directories:
> >      - path: "/app/APTEST/application"
> >        owner: "user_one"
> >        group: "user_one"
> >        mode: 755
> >      - path: "/app/APTEST/log"
> >        owner: "user_one"
> >        group: "user_one"
> >        mode: 755
> >      - path: "/app/APTEST/data"
> >        owner: "user_one"
> >        group: "user_one"
> >        mode: 755
> > ```
> > ##### `application_directories.path`  **This var is mendatory !**
> > The directory path. <br>
> > 
> > ##### `application_directories.owner` **This var is mendatory !**
> > The directory owner. <br>
> >
> > ##### `application_directories.group` **This var is mendatory !**
> > The directory group. <br>
> >
> > ##### `application_directories.mode` **This var is mendatory !**
> > The directory mode. <br>
> > <br>
> >
> > 
> > ##### `application_lvms_create` **This var is mendatory !**
> > This bolean variable used to enable or disable lvm creation.<br>
> > This variable should be declare on inventory `group_vars`.<br>
> > **This variable is mendatory !** <br> 
> > **If this variable is set to `true` you must set the `application_vgs` Dictionary !**<br>
> > <br>
> > 
> > 
> > #### The `application_vgs` Dictionary
> > ---
> > This dictionary gathers all Application **Volume Groupe's (VGs)** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > Volumes groups are created by picking disk's path on `disks` dictonaries if disks.tag equal to vgs.tag.
> > **This dictionary is mendatory if `application_lvms_create` is set to `true` !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >    application_vgs:
> >      - name : "vg_app"
> >        tag: "APTEST"
> >        lvs :
> >        - name: "lv_app"
> >          size: '10g'
> >          fstype: 'xfs'
> >          owner: "user_one"
> >          group: "user_one"
> >          mode: 755
> >          mount_point: /app/APTEST/application
> >        - name: "lv_log"
> >          size: '10g'
> >          fstype: 'xfs'
> >          owner: "user_one"
> >          group: "user_one"
> >          mode: 755
> >          mount_point: /app/APTEST/log
> >        - name: "lv_data"
> >          size: '10g'
> >          fstype: 'xfs'
> >          owner: "user_one"
> >          group: "user_one"
> >          mode: 755
> >          mount_point: /app/APTEST/data
> > ```
> > <br>
> > 
> > 
> > #### The `disks` Dictionary
> > ---
> > This dictionary gathers all host **Physical volumes** variables.<br>
> > This dictionary should be declare on inventory `host_vars`.<br>
> > **This dictionary is mendatory if `application_lvms_create` is set to `true` !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*) <br>
> >
> > ```yaml
> >    disks:
> >      - path: '/dev/disk/azure/scsi1/lun1'
> >        tag: APTEST
> >      - path: '/dev/sdc'
> >        tag: APTEST
> >      - path: '/dev/sde'
> >        tag: AFFFF
> > ```
> > <br>
## Dependencies

- ansible.builtin
- ansible.posix

## Example Playbook

```yaml
    - hosts: server
      roles:
        - { role: shmii.app-common }
```
# Test role with `molecule`

## Pre-requists



## Test role commande


```bash
RHEL_ACCOUNT_USER='<MyRedhatAccountUser>' RHEL_ACCOUNT_PASSWORD='<MyRedhatAccountPassword>'  molecule check
```


## Warning and known bugs

/!\ To validate this role with "molecule" from Ubuntu @ WSL/WSL2 it is necessary to create the folder  `/sys/fs/cgroup/systemd` on the host linux subsystem. This directory is necessary for some os (RHEL9, Rocky9, etc...) and not existing on local Ubuntu WSL sub Systeme !

```console
$ sudo mkdir /sys/fs/cgroup/systemd
```

## Sources and Bibliography

### Sources
Redde Caesari quae sunt Caesaris, et quae sunt Dei Deo !
 
- Jeff GEERLING ( https://www.linkedin.com/in/jeff-geerling-086bb2a/  --- https://github.com/geerlingguy)
- Stephane ROBERT ( https://www.linkedin.com/in/stephanerobert1/  ---  https://blog.stephane-robert.info)

### Bibliography
- Ansible - Automation for everyone : ( https://www.ansible.com )
- Ansible - Ansible Documentation : ( https://docs.ansible.com )
- Molecule - Ansible Molecule Documentation : ( https://molecule.readthedocs.io/en/latest/ )
- Molecule - Ansible Molecule Community Documentation : ( https://github.com/ansible-community/molecule)
- Molecule/Ansible :  Stephane Robert's Blog : ( https://blog.stephane-robert.info/tags/ansible/ )

## License

GNU GENERAL PUBLIC LICENSE Version 3, 29 June 2007

## Author Information

This role was created in 2022 by [Thomas CHALMEL] (https://www.thomas.chalmel.org/).
