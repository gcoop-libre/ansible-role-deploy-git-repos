Deploy git repos
================

Deploy a list of git repositories on the target host.

The role also can set the permissions of the uploaded files.

Requirements
------------

`rsync` should be available on the target host.

If you would execute the remote `rsync` as root (setting `deploy_git_repos_become_rsync` as `yes`) or you need to change the permissions of the uploaded files, the remote login user should be granted to execute `rsync` with sudo.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    deploy_git_repos_temp_directory_mask: /tmp/deploy_git_repos.XXXXXXXXXX

This property sets the mask used to create the local temporal directory where the repository will be cloned.

    deploy_git_repos_become_rsync: no

When this property is set to `yes`, the synchronize task will use `sudo rsync` as remote rsync command.

    deploy_git_repos:
      - name: Test
        repo: git@github.com:gcoop-libre/ansible-role-deploy-git-repos.git
        version: master
        dest: /var/www
        subtree: app
        excludes:
          - .git*
        clean: false
        perms:
          - path: cache
            owner: www-data
            group: www-data
            mode: '0775'
            recurse: false

List of repositories to deploy on the target host. `name`, `repo` and `dest` are mandatory. `exclude` can be a list of files or patterns to exclude when uploading the cloned repository to the target host.

Enabling `clean` will delete all the files on `dest` which are not on the repository. `perms` is a list of files or directories, which owners and permissions should be modified. `subtree` is used to deploy only a subtree of the cloned repository (optional).

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
         - gcoop-libre.deploy-git-repos

*Inside `vars/main.yml`*:

    deploy_git_repos:
      - name: Test
        repo: https://github.com/gcoop-libre/ansible-role-deploy-git-repos.git
        dest: ~/test
        excludes:
          - .git*

License
-------

GPLv2

Author Information
------------------

This role was created in 2016 by [gcoop Cooperativa de Software Libre](https://www.gcoop.coop).
