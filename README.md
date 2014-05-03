Logical MySQL Manager
=====================

A set of scripts for branching and switching active MySQL databases using LVM.

Primary Features
----------------

* Instant cloning of MySQL databases.
* Instant switching of the active MySQL database.
* Automatic handling of LVM snapshot management.

Examples
--------

```bash
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm list
Active snapshot: /mysql/master

Database snapshots:
  master
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm branch fancy-feature
  Logical volume "fancy-feature" created
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm list
Active snapshot: /mysql/master

Database snapshots:
  fancy-feature
  master
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm switch fancy-feature
/mysql/master is the currently active database.
 * Stopping MariaDB database server mysqld                                       [ OK ]
Setting /mysql/fancy-feature as the active database.
 * Starting MariaDB database server mysqld                                       [ OK ]
 * Checking for corrupt, not cleanly closed and upgrade needing tables.
vagrant@vagrant-ubuntu-trusty-32:~$ echo 'CREATE DATABASE feature;' | mysql -u root
vagrant@vagrant-ubuntu-trusty-32:~$ echo 'SHOW DATABASES;' | mysql -u root
Database
information_schema
feature
lost+found
mysql
performance_schema
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm switch master
/mysql/fancy-feature is the currently active database.
 * Stopping MariaDB database server mysqld                                       [ OK ]
Setting /mysql/master as the active database.
 * Starting MariaDB database server mysqld                                       [ OK ]
 * Checking for corrupt, not cleanly closed and upgrade needing tables.
vagrant@vagrant-ubuntu-trusty-32:~$ echo 'SHOW DATABASES;' | mysql -u root
Database
information_schema
lost+found
mysql
performance_schema
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm list
Active snapshot: /mysql/master

Database snapshots:
  fancy-feature
  master
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm destroy fancy-feature
Do you really want to remove and DISCARD active logical volume fancy-feature? [y/n]: y
  Logical volume "fancy-feature" successfully removed
vagrant@vagrant-ubuntu-trusty-32:~$ sudo lmm list
Active snapshot: /mysql/master

Database snapshots:
  master
* branch
* list
* switch
* destroy
vagrant@vagrant-ubuntu-trusty-32:~$