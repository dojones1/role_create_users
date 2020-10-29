Role Name
=========

An ansible role that sets up deployment, admin and normal user accounts with public keys on linux

Three classes of account are defined:
- deploy_admins: Can connect with a public SSH key and have membership of sudo / wheel and deploy group
- admins: Can connect with public SSH key or created with a temporary password. The password file will be stored in a credentials folder adjacent to the playbook. They will have membership of sudo/wheel group
- users: Can connect with a public SSH key or a temporary password. The password file will be stored in a credentials folder adjacent to the playbook.

Each account class can be assigned to a different set of groups
Passwords are automatically given an expiry date
The role can also delete the original user account (e.g. a default ubuntu account or vagrant) in favour of a deploy_admin account
The assumption is that the create_users_deploy_user is in the create_users_deploy_admins list

Requirements
------------

None

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`)

  ---
  # defaults file for role_create_users

  # create_users_deploy_user:
  # Default user for deployment - This is a mandatory parameter - This should be vagrant if running from vagrant boxes

  create_users_login_shell: "/bin/bash"
  # Default shell to be assigned to users

  create_users_password_expiry_days: 365
  # Default duration for user passwords

  create_users_allow_passwordless_sudo: true
  # Enables passwordless sudo
  # Note: This role will not toffle the flag if the inventory value is changed after initial execution on the host

  # The following variables should be overwritten
  # create_users_deploy_admins: {}
  # Should be of the form:
  # create_users_deploy_admins:
  #   <user>:
  #     sshpubkey:

  create_users_admin_groups_base: "adm,cdrom"
  create_users_deploy_admins_groups: "{{ create_users_admin_groups_os }},deploy"
  # Groups to be granted to deploy admins

  # create_users_admins: {}
  # Should be of the form:
  # create_users_admins:
  #   <user>:
  #     sshpubkey: "ssh-rsa BLAHBLAHBLAH <user>"

  create_users_admins_groups: "{{ create_users_admin_groups_os }}"
  # Groups to be joined for normal admins

  # create_users_users: {}
  # Should be of the form:
  # create_users_users:
  #   <user>:
  #     sshpubkey: "ssh-rsa BLAHBLAHBLAH <user>"

  create_users_users_groups:
  # Groups to be joined for normal users
  ...

Dependencies
------------

None

Example Playbook
----------------

```yaml
    - hosts: all
      vars:
        create_users_deploy_user: vagrant
      roles:
         - role_create_users
```

Author Information
------------------

Donald Jones