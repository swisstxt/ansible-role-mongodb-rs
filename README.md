# Ansible MongoDB Role

An ansible role for installing and managing MongoDB replica sets's on CentOS.

## Dependencies

None.

## Example Hosts File

```
[mongo-rs-testrs]
mongo-01.domain.local mongo_node_name=mongo-01.example.com mongo_init_node=true
mongo-02.domain.local mongo_node_name=mongo-02.example.com
mongo-03.domain.local mongo_node_name=mongo-03.example.com mongo_backup_source=true mongo_arbiter=true
```

## Example Group Vars

```
---
mongo_replica_set: testrs

mongo_admin_user: admin
mongo_admin_pass: supersavepw

mongo_databases:
- name: testdatabase
  users:
  - name: testuser
    pass: testpw
    roles:
    - role: dbOwner
      db: testdatabase
```

## Example Playbook

```yaml
---
- hosts: mongo-rs-*
  remote_user: root

  roles:
  - mongodb

  pre_tasks:
  - name: get mongo_group_name
    set_fact: mongo_group_name="{{ item }}"
    with_items: group_names
    when: item | search("mongo-rs-*")

  - name: get mongo_replica_set
    set_fact: mongo_replica_set="{{ mongo_group_name }}"
    when: mongo_replica_set is not defined
```

## License

MIT

## Author Information

Daniel Menet
