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

> ### Requiered variables
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
> >
> > #### The `application_groups` dictionaries.
> > ---
> > This dictionary gathers all Application **Groups** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory !** <br> 
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
> > This dictionary gathers all Application **Directories** variables.<br>
> > This dictionary should be declare on inventory `group_vars`.<br>
> > **This dictionary is mendatory !** <br> 
> > (*This dictionary vars should normaly be provided by the Configuration management database (CMDB)*)
> > ```yaml
> >    application_directories:
> >      - path: "/app/{{ application[0].tag }}/application"
> >        owner: "{{ application[0].id }}"
> >        group: "{{ application[0].id }}"
> >        mode: 755
> >      - path: "/app/{{ application[0].tag }}/log"
> >        owner: "{{ application[0].id }}"
> >        group: "{{ application[0].id }}"
> >        mode: 755
> >      - path: "/app/{{ application[0].tag }}/data"
> >        owner: "{{ application[0].id }}"
> >        group: "{{ application[0].id }}"
> >        mode: 755
> > ```




```yaml
application_lvms_create: true 
application_lvms:
  - vg_name : "vg_app"
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
```





> > Use `application_lvm_create` to chose if you wanted to create dedicate VGs and LVs for your application. This var is set to `false` by default. <br>
> > Use below vars to configure LVM, and there FilesSysteme
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


```yaml
servers:
  - name : VM1
    disks:
      - name : hdd1
        type : ssd
        tag: APTEST
        size: 10G
        path: '/dev/disk/azure/scsi1/lun1'
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
