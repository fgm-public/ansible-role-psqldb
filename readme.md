# Role for databases provisioning on testing stages

Role supports some `actions` listed below:

* Create databases (`create`)
* Drop databases (`drop`)
* Dump databases (`dump`)
* Restore databases (`restore`,`replace`)
* Install extensions on databases (`extend`)
* Collect simple info about databases and clusters (`stat`,`list`)

## Sample playbook

---

    # - hosts: "{{ nodes }}"
    - hosts: postgres
      gather_facts: true
      become: yes
      become_user: postgres
      roles:
        - role: service_db


Role behavior (which `actions` listed above will be taken as part of the next play) depends on ansible `--tags` parameter passed to play:

    ansible-playbook postgres.yml --tags replace

The snippet above will start database replacement process which will replace existing databases (defined in `db_list['db_name']` variable ) with databases restored from dump (defined in `db_list['source_name']`)

All role specific parameters (database names, it's related attributes, filesystem paths for backup and so on) defined in `vars/main.yml`. Thus, it's the only one place to put your hands on play customization.

## ~800 lines

<!-- # - name: Simulate long running op (15 sec), wait for up to 45 sec, poll every 5 sec
#   ansible.builtin.command: /bin/sleep "{{ item.delay }}"
#   # Task timeout in seconds, if elapsed, then task will fail with message:
#   # "msg": "async task did not complete within the requested time - 15s"
#   async: 86400 # Day
#   # If you want to run multiple tasks in a playbook concurrently, use async with poll set to 0. 
#   # When you set poll: 0, Ansible starts the task and immediately moves on to the next task without waiting for a result.
#   poll: 0 # Async
#   loop: "{{ db_list }}"
#   loop_control:
#     label: "{{ item.db_name }}"
#   register: async_results
#   tags:
#     - create
#     - replace -->