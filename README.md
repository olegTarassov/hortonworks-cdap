hortonworks-cdap
================

Install CDAP on top of Ambari HDP 2.6 from HortonWorks.

Role performs the following:

- Install CDAP on the Master node
- Sets HDFS directories for yarn and cdap user
- Configures ranger permissions for hdfs, hbase and kafka services

Requirements
------------

Builds on top of [ansible-hortonworks](https://github.com/hortonworks/ansible-hortonworks) to install CDAP Service and its components. Preferable to have access to the instance in passwordless by setting `ansible_ssh_private_key_file= ssh_key_path`.

Role Variables
--------------

| Variable name                     | default                                                                      | Notes                                                                |
| --------------------------------- | ---------------------------------------------------------------------------- | :------------------------------------------------------------------- |
| ambari_master_node                | hdp-master                                                                   | nonha ambari node is also master node and cdap instance              |
| cdap_version                      | 5.1                                                                          | Supported on 5.x only                                                |
| hdp_config.zoo.cfg.maxClientCnxns | "0"                                                                          | Requirement by CDAP                                                  |
| cdap_components                   | [ CDAP_MASTER, CDAP_ROUTER, CDAP_UI, CDAP_KAFKA, CDAP_AUTH_SERVER, CDAP_CLI] | CDAP Mandatory components                                            |
| cdap_path_policy_config           | {}                                                                           | CDAP Ranger policy configuration                                     |
| yarn_path_policy_config           | {}                                                                           | YARN Ranger policy configuration                                     |
| hbase_all_policy_config           | {}                                                                           | HBASE Ranger policy configuration                                    |
| kafka_brokers_policy_config       | {}                                                                           | KAFKA Ranger policy configuration                                    |
| kafka_atlas_policy_config         | {}                                                                           | KAFKA Ranger policy configuration                                    |
| private_repo_url                  | omit                                                                         | Use Private Repos instead of Public. URL that contains CDAP packages |

The following variables are normally set in the project [ansible-hortonworks-all](https://github.com/hortonworks/ansible-hortonworks/blob/master/playbooks/group_vars/all) but for clarity are defined in default as well.

| Variable name                                 | default       | Notes                                                |
| --------------------------------------------- | ------------- | :--------------------------------------------------- |
| cluster_name                                  | mytestcluster | Need to Overwrite this or all ambari tasks will fail |
| ambari_admin_user                             | admin         |                                                      |
| ambari_admin_password                         | admin         |                                                      |
| ranger_security_options.ranger_admin_password | admin         |                                                      |

Dependencies
------------

Python:

- ansible

Services:

- ambari
- ranger
- hdfs
- yarn
- hbase
- kafka

Inventory:

```ini
[hdp-master]
FQDN_NONHA_MASTER_NODE

[hdp-slave]
FQDN_SLAVE_NODE_01
FQDN_SLAVE_NODE_02
FQDN_SLAVE_NODE_03
```

Limitations
-----------

- A nonha Environment
- HDP version 2.x
- CDAP version 5.x

Example Playbook
----------------

**install_cdap.yml**

```yaml
- name: Apply the cdap role
  hosts: hdp-master
  any_errors_fatal: true
  roles:
    - olegtarassov.ambari_cdap
```

```bash
ansible-playbook -i inventory/static  playbooks/install_cdap.yml -diff
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
