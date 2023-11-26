# Ansible

Production ready PostgreSQL cluster with ETCD.

Software Version

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

# play with cluster

create or delete test databases etc...

```watch 'psql -c "\l"'```

```sudo patronictl -c /etc/patroni/patroni.yml list```

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



