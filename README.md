# Maven Role

## Introduction

This role installs apache maven and makes it available to be used by all users on the system.

## Role Depenendencies

This role requires java to be installed - it currently depends on [geerlingguy.java](https://github.com/geerlingguy/ansible-role-java) and installs OpenJDK 8.

## Role Variables

### Defaults

All role default variables can be seen in the [defaults/main.yml file](https://github.com/oldNoakes/ansible-role-maven/blob/master/defaults/main.yml).

Variables that are of interest to end users are:

```
maven_user: "root"                  # If maven is being setup to be used by a given user (e.g. jenkins), this will ensure the settings.xml file is put in their home
maven_install_dir: "/usr/local"     # Where should maven be installed into
maven_version: "3.5.2"              # Version of Maven to install - see available versions at https://archive.apache.org/dist/maven/maven-3/
```

### Custom variables

A custom [Maven settings file](https://maven.apache.org/settings.html) can be used by injecting the complete XML via the ```maven_settings_file_content``` variable.  If this is not provided, the [default settings.xml file](https://github.com/oldNoakes/ansible-role-maven/blob/master/files/settings.xml) is used.

Here is an example of setting up a custom settings file via variable injection:

```yaml
    - role: ansible-role-maven
      maven_settings_file_content: |
        <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                              http://maven.apache.org/xsd/settings-1.0.0.xsd">
          <interactiveMode>true</interactiveMode>
        </settings>
```

## Role Usage

### Ansible Galaxy Requirements.yml

* Add the following content into the requirements.yml (Ansible Galaxy file).  If you need a specific version, add that based on the [available tagged versions](https://github.com/oldNoakes/ansible-role-maven/tags) 

```yaml
---
# from Github with specific tagged version and rename
- src: https://github.com/oldNoakes/ansible-role-maven.git
  name: maven
  version: 1.0.0

# from Galaxy with specific tagged version
- src: oldNoakes.maven
  version : 1.0.0

# from Galaxy 
- src: oldNoakes.maven
```

### Role usage in playbook

* Use the role in your playbook:

```yaml
- hosts: servers
  roles:
    - oldNoakes.maven
```