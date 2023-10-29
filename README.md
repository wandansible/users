Ansible role: Users
===================

Create local user accounts and groups.

Role Variables
--------------

```
ENTRY POINT: main - Create users and groups

        Create local user accounts and groups.

OPTIONS (= is mandatory):

- root_password_hash
        If provided, set the root user's password to the provided
        encrypted hash. Password hash can be generated with the
        `mkpasswd' command. To lock the root account password on Linux
        systems, set this to '!' or '*'.
        default: null
        type: str

- user_groups
        List of user groups to create
        default: []
        elements: dict
        type: list

        OPTIONS:

        - gid
            GID to set for the group
            default: null
            type: int

        = name
            Name of the group to create
            type: str

        - system
            If true, makes the group a system group
            default: null
            type: bool

- user_shell_packages
        List shell packages to install
        default: null
        elements: str
        type: list

- users
        List of user accounts to create
        default: []
        elements: dict
        type: list

        OPTIONS:

        - append
            If true, add the user to the groups specified in groups.
            If false, user will only be added to the groups specified
            in groups, removing them from all other groups.
            default: null
            type: bool

        - authorized_keys
            List of SSH public keys to add to authorized_keys file
            default: null
            elements: str
            type: list

        - authorized_keys_exclusive
            If true, remove all other non-specified SSH public keys
            from the authorized_keys file
            default: false
            type: bool

        - create_home
            Unless set to false, a home directory will be made for the
            user when the account is created, if the home directory
            does not already exist
            default: null
            type: bool

        - expires
            Date in Unix timestamp format when the user account will
            be disabled
            default: null
            type: float

        - generate_ssh_key
            If true, generate a SSH key for the user in question
            default: null
            type: bool

        - group
            Group to set as the user's primary group
            default: null
            type: str

        - groups
            List of groups user will be added to
            default: null
            elements: str
            type: list

        - home
            Set the user's home directory
            default: null
            type: str

        - name
            Sets the description (aka GECOS) of the user account
            default: null
            type: str

        - password
            If provided, set the user's password to the provided
            encrypted hash. Password hash can be generated with the
            `mkpasswd' command. To create an account with a
            locked/disabled password on Linux systems, set this to '!'
            or '*'.
            default: null
            type: str

        - password_expire_max
            Maximum number of days between password change
            default: null
            type: int

        - password_expire_min
            Minimum number of days between password change
            default: null
            type: int

        - password_lock
            If true, lock the password for the account
            default: null
            type: bool

        - shell
            Set the user's shell
            default: null
            type: str

        - ssh_key_bits
            Specify the number of bits in the created SSH key when
            generate_ssh_key is true
            default: null
            type: int

        - ssh_key_comment
            Specify the comment for the created SSH key when
            generate_ssh_key is true
            default: null
            type: str

        - ssh_key_passphrase
            Specify the passphrase for the created SSH key when
            generate_ssh_key is true
            default: null
            type: str

        - ssh_key_type
            Specify the key type for the created SSH key when
            generate_ssh_key is true
            default: ed25519
            type: str

        - system
            If true, makes the user a system account
            default: null
            type: bool

        - uid
            Set the UID of the user account
            default: null
            type: int

        - umask
            Set the umask of the user
            default: null
            type: str

        - update_password
            If set to always, will always update passwords if they
            differ. If set to on_create, will only set the password
            for newly created users.
            choices: [always, on_create]
            default: null
            type: str

        = username
            Name of the user to create
            type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/users,main,wandansible.users
     
Or, by adding the following to `requirements.yml`:

    - name: wandansible.users
      src: https://github.com/wandansible/users

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: all
      roles:
        - role: wandansible.users
          become: true
          vars:
            root_password_hash: "!"

            users:
              - username: example
                name: Example User
                uid: 5000
                groups: ["sudo"]
                append: true
                authorized_keys:
                  - "ssh-ed25519 AAAAAAA example@example-host"

              - username: example2
                name: Example User 2
                password: $6$0e7w/A.f6lZ8CCId$/l8IatE7Nbl9iD7ylEJRpuHkwIZc2iE9T6m1zcx0KvAiQ12FHkGI0qIZp74apeRzwuDk3BGJOmSqC1zhvP3iM1
                group: example-user-group
                authorized_keys:
                  - "ssh-ed25519 BBBBBBB example2@example-host"
                  - "ssh-ed25519 CCCCCCC example2@example-host"
                authorized_keys_exclusive: true
                generate_ssh_key: true
                ssh_key_comment: example2@example-host
                shell: /bin/bash

              - username: example-system
                name: Example System User
                system: true
                group: example-system
                home: /var/lib/example

            user_groups:
              - name: example-user-group
                gid: 1234

              - name: example-system
                system: true
