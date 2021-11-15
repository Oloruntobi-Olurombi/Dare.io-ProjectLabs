# Ansible Dynamic Assignments (Include) and Community Roles

### Please note that this is a continuation of project 12.

In our https://github.com/<your-name>/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.
- git checkout -b dynamic-assignments
  
Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml.

We will instruct site.yml to include this playbook later. For now, we would keep building up the structure.
  
![image](https://user-images.githubusercontent.com/40290711/141774057-ddd5c7e7-c69b-45e1-93d5-41acbc59f5ce.png)

Our GitHub shall have following structure by now:
  
├── dynamic-assignments
  
│   └── env-vars.yml
  
├── inventory
  
│   └── dev
    └── stage
    └── uat
    └── prod
  
└── playbooks
  
    └── site.yml
  
└── roles (optional folder)
  
    └──...(optional subfolders & files)
  
└── static-assignments
  
    └── common.yml

Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc., we will need a way to set values to variables per specific environment.

For this reason, we will now create a folder to keep each environment’s variables file. Therefore, create a new folder env-vars, then for each environment, create new YAML files which we will use to set variables.

Our layout should now look like this:

  
├── dynamic-assignments
  
│   └── env-vars.yml
  
├── env-vars
  
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
  
├── inventory
  
    └── dev
    └── stage
    └── uat
    └── prod
  
├── playbooks
  
    └── site.yml
  
└── static-assignments
  
    └── common.yml
    └── webservers.yml
 
![image](https://user-images.githubusercontent.com/40290711/141775276-b9f9702d-ce78-40a3-87ae-273007342fe7.png)
  
Now paste the instruction below into the env-vars.yml file:
  
 ``` 

---
- name: collate variables from env specific file, if it exists
  include_vars: "{{ item }}"
  with_first_found:
    - "{{ playbook_dir }}/../env_vars/{{ "{{ inventory_file }}.yml"
    - "{{ playbook_dir }}/../env_vars/default.yml"
  tags:
    - always

```

![image](https://user-images.githubusercontent.com/40290711/141776851-ffedc3bc-9f71-4d68-a454-45c044c9fe76.png)
  
Notice 3 things to note here:

We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module.

From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:

- include_role
- include_tasks
- include_vars
  
In the same version, variants of import were also introduces, such as:

- import_role
- import_tasks
 
 
We made use of special variables {{ playbook_dir }} and {{ inventory_file }}. {{ playbook_dir }} will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem.

{{ inventory_file }} on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.

We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.
  
#### Update site.yml with dynamic assignments
  
Update site.yml file to make use of the dynamic assignment. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come)

site.yml should now look like this: 

```
---
- name: Include dynamic variables 
  hosts: all
  tasks:
    - import_playbook: ../static-assignments/common.yml 
    - include_playbook: ../dynamic-assignments/env-vars.yml
  tags:
    - always

- name: Webserver assignment
  hosts: webservers
    - import_playbook: ../static-assignments/webservers.yml
``` 
