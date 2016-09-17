# ansible-deploy
[![Build Status](https://travis-ci.org/jkanclerz/ansible-deploy.svg?branch=master)](https://travis-ci.org/jkanclerz/ansible-deploy)

Ansible abstraction for deploy tasks.


# Ansible Deploy structure

Allows to deploy project dist with various methods. Contains reference to three main roles

- [Deploy setup](https://github.com/jkanclerz/ansible-deploy-setup)
- [Deploy project](https://github.com/jkanclerz/ansible-deploy-project)
- [Deploy finalize](https://github.com/jkanclerz/ansible-deploy-finalize)

## Role Variables

Available variables are listed below, along with example values (see `deploy.yml`):

    deploy:
      project:
        root: '/tmp/test_deploy'
        name: 'My Project'
        scm: 'rsync'
        source: "dist"
        keep_previous_releases: 3
        copy_previous_release: True
        writable_dirs:
          - src: "var/files"
        shared_dirs:
          - "var/files"
        shared_files: []
        templated_files:
          -
            source: "config/settings.ini.j2"
            dest: "config/settings.ini"
            owner: "{{ ansible_user }}"
            group: "{{ ansible_user }}"
      finalize:
        required_sudo: False
        commands: []
        to_be_restarted_services: []
    permission_model: 'chmod'


    deploy.project.root: '/tmp/test_deploy'

Destination project directory.

    deploy.project.name: 'My project'

Identifies project name

    deploy.project.scm: 'rsync'

Identifies project deploy method. Could be choosen from (rsync|git). Make sure your target meets software requirements. git or rsync are instaled.


	deploy.project.source: 'dist'

Identifies project distribution source. Could be relative or absolute ('/home/jkanclerz/projects/foo/dist')

	deploy.project.keep_previous_releases: 3

Identifies how many previous releases are kept on your target. Role will remove all other previous releases. Keep just choosen amount.

	deploy.project.copy_previous_release: True

Force to copy currently used release as copy that is updated at code update stage. Increase speed of code updating. If switched off then builded dist is fully copied as next release.


	deploy.project.writable_dirs:
		- src: "var/files"

Allows define which directories should be writable for web server user. 

	deploy.project.shared_dirs:
    	- "var/files"

Allows define which directories should be shared between releases

	deploy.project.shared_files:
    	- "config/settings.yml"

Allows define which files should be shared between releases

	deploy.project.templated_files:
        -
            source: "config/settings.ini.j2"
            dest: "config/settings.ini"
            owner: "{{ ansible_user }}"
            group: "{{ ansible_user }}"

Allows define which files should be copied from local stored template.

	deploy.finalize.required_sudo: True

Define if sudo is required to perform finalization tasks

	deploy.finalize.commands:
		- 'echo "deploy finished"'

Define define commands to be executed after finished deploy


	deploy.finalize.to_be_restarted_services:
		- 'nginx'
		- 'my_app_service'

Define define services that should be restarted after finished deploy


# Ansible Rollback

Allows to rollback project deployed via above roles

- [Deploy rollback](https://github.com/jkanclerz/ansible-deploy-rollback)

## Role Variables

Available variables are listed below, along with example values (see `deploy.yml`):

    deploy:
      project:
        root: '/tmp/test_deploy'


    deploy.project.root: '/tmp/test_deploy'

Destination project directory.

## License

MIT / BSD

## Author Information

Created by [Jakub Kanclerz](http://jkan.pl)