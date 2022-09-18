
# Ansible role for postgres databases provisioning

Role supports some `actions` listed below:

* (`create`)  Create new databases 
* (`restore`) Restore databases from dumps in async mode
* (`replace`) Replace existing databases from dumps in async mode
* (`extend`)  Install extensions on databases
* (`drop`)    Drop existing databases
* (`dump`)    Dump existing databases in async mode
* (`stat`)    Collect simple info about databases
* (`list`)    Collect simple info about clusters

Role behavior (which `actions` listed above will be taken as part of the next play) depends on ansible `--tags` parameter passed to play, for example:

    ansible-playbook postgres.yml --tags replace

The snippet above will start database replacement process which will replace existing databases (defined in `db_list['db_name']` variable) with databases restored from dump (defined in `db_list['source_name']`)

All role specific parameters (database names, it's related attributes, filesystem paths for backup and so on) defined in `vars/main.yml`. Thus, it's the only one place to put your hands on play customization.

## Sample playbook

    ---
    - hosts: dev
      gather_facts: true
      become: yes
      become_user: postgres
      roles:
        - role: service_db
