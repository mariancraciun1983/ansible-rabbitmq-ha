<h1 align="center">RabbitMQ HA</h1>
<br />

<div align="center">
  <a href="https://travis-ci.com/mariancraciun1983/ansible-rabbitmq-ha">
    <img src="https://travis-ci.com/mariancraciun1983/ansible-rabbitmq-ha.svg?branch=master" alt="Build Status" />
  </a>
  <a href="https://galaxy.ansible.com/mariancraciun1983/rabbitmq_ha">
    <img src="https://img.shields.io/ansible/role/51705" alt="Ansible Galaxy" />
  </a>
  <a href="https://galaxy.ansible.com/mariancraciun1983/rabbitmq_ha">
    <img src="https://img.shields.io/ansible/quality/51705" alt="Ansible Quality Score" />
  </a>
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="MIT License" />
  </a>
</div>
<br />



## Introduction

  This ansible role provides the basic installation and setup for RabbitMQ plus the Cluster and HA configuration.
  In cluster mode, one of the nodes need to be configured as master and any additional one as slave.


## High Availability

 High Availability is achieved with the ha-mode policy, which can be fine tuned.
```json
{
  "ha-mode":"exactly",
  "ha-sync-mode":"automatic",
  "ha-sync-batch-size":50000,
  "ha-params":2
}
```

  Whenever a queue is defined, it will persist on 2 nodes (ha-params) and when one of the nodes goes down,
  the queue will be reballanced onto the other node (if there is any). 
  The ha-sync-batch-size configures the batch size for synchronisation.

## Configuration example

For a single node

```yaml
rabbitmq_role: master
rabbitmq_user:
  username: user
  password: pass
```

For a cluster with HA
```yaml
group_vars:
  all:
    # enable HA
    rabbitmq_ha: true
    # configure default user
    rabbitmq_user:
      username: user
      password: pass
host_vars:
  node1:
    rabbitmq_role: master
  node2:
    rabbitmq_role: slave
  node3:
    rabbitmq_role: slave
```

Additional special config vars:
```yaml
# edit the /etc/hosts files and ensure all rabbitmq hosts are present
rabbitmq_exchange_hosts: true
# use a variable called internal_ip for the host ip when exchanging host names
rabbitmq_exchange_hosts_internal_ip: true
```

## Testing

Molecule with docker is being used.

Running the tests:
```bash
pipenv install
pipenv run molecule test
```

## License

MIT License