# Ansible

**Production ready PostgreSQL cluster with ETCD.**

**Software Version**

OS: ```Ubuntu 22.04```

PostgreSQL: ```15.5```


# Hosts

```192.168.1.110``` - lb

```192.168.1.111``` - master

```192.168.1.112``` - replica

```192.168.1.113```  - replica

# etcd nodes

```192.168.1.111```

```192.168.1.112```

```192.168.1.113```

# Installation
```ansible-playbook deplog_pgcluster.yml -bkK```

# check cluster nodes
```sudo patronictl -c /etc/patroni/patroni.yml --help```

```sudo patronictl -c /etc/patroni/patroni.yml list```


```
+ Cluster: postgres-cluster (7305439748098054840) ----+----+-----------+
| Member   | Host          | Role         | State     | TL | Lag in MB |
+----------+---------------+--------------+-----------+----+-----------+
| pgnode01 | 192.168.1.111 | Sync Standby | streaming |  5 |         0 |
| pgnode02 | 192.168.1.112 | Leader       | running   |  5 |           |
| pgnode03 | 192.168.1.113 | Replica      | streaming |  5 |         0 |
+----------+---------------+--------------+-----------+----+-----------+
```


# play with cluster

create or delete test databases etc...


```
create database delivery;

create user nurlan;

GRANT ALL PRIVILEGES ON DATABASE delivery TO nurlan;

ALTER USER nurlan WITH PASSWORD 'qwerty123';
```

Connection test via DBeaver

![image](https://github.com/Nurlan199206/patroni_cluster/assets/22808731/96687d04-d984-4a74-9994-081a85f12f83)



```watch 'psql -c "\l"'```


# Manual switchover
```sudo patronictl -c /etc/patroni/patroni.yml switchover```


```primary [node2]: node2
Candidate ['node1', 'node3'] []: node1
When should the switchover take place (e.g. 2021-09-23T15:56 )  [now]: now
Current cluster topology
+ Cluster: stampede1 (7011110722654005156) -----------+
| Member | Host  | Role    | State   | TL | Lag in MB |
+--------+-------+---------+---------+----+-----------+
| node1  | node1 | Replica | running |  3 |         0 |
| node2  | node2 | Leader  | running |  3 |           |
| node3  | node3 | Replica | stopped |    |   unknown |
+--------+-------+---------+---------+----+-----------+
Are you sure you want to switchover cluster stampede1, demoting current primary node2? [y/N]: y
2021-09-23 14:56:40.54009 Successfully switched over to "node1"
+ Cluster: stampede1 (7011110722654005156) -----------+
| Member | Host  | Role    | State   | TL | Lag in MB |
+--------+-------+---------+---------+----+-----------+
| node1  | node1 | Leader  | running |  3 |           |
| node2  | node2 | Replica | stopped |    |   unknown |
| node3  | node3 | Replica | stopped |    |   unknown |
+--------+-------+---------+---------+----+-----------+
```

# troubleshoot

```sudo journalctl -u patroni.service -n 100 -f```

# dynamically edit config

```patronictl -c /etc/patroni/prod-hm-postgresql-03.yml edit-config```


