cluster-in-a-box
================

Creates and manage a mongodb cluster (a sharded group of nodes, each one a replicaset with at least three mongod instances) in the local machine.
Provides an easy way to administrate it as a group or individually, to provide a learning sandbox for developers and DBAs as well.

Purpose of the tool:
To create and configure a cluster with 'n' shards ('n' configurable, default for learning processes at this time, 2),
each shard composed of a replicaset with three servers (two mongod and one arbiter).
It creates them in the local machine with a different port for each one of the parts.
By default, the mongos process is going to be attached to the default port of mongodb: 27017

You can use the tool to 'start', 'stop', and 'kill' the cluster. You can also get the status and do the initial configuration.

You can get the source code here:
https://github.com/sroysen/at-the-cold

You need to install pymongo (the python mongodb driver).

You are also going to need to download a proper binary distribution of mongodb for your machine and store the .tgz file
in the same folder where you are planning to run the tool the first time.
http://www.mongodb.org/downloads
At this time, it only runs under Linux (tested under 32/64 bits of Centos, OpenSUSE and Ubuntu).

To keep disk usage to a minimum, it will not preallocate space in the way the usual mongodb installations do.

The easy installation guide (none of this should be run as root, there is no need for it).

1) Create a proper folder to keep everything together
mkdir -p $HOME/Projects/mongocluster ; cd $HOME/Projects/mongocluster

2) Get the python script from github (cluster_in_a_box_mongodb.py) and copy it to the working folder you created in 1)

3) Download the version of mongodb that you need for your platform and copy the .tgz file to the working folder you created in 1)

4) Run the script with the 'create' command to create the appropriate structure of folders and files (it's going to use the default name of 'localCluster'):
./cluster_in_a_box_mongodb.py create

It will create the following folders and populate them with appropriate contents (logs, config files, init scripts, binaries)
localCluster/bin (symlinks to the actual binaries that are going to be used)
localCluster/data (a parent folder for all the data folders that are going to be required)
localCluster/etc (the folder for the individual configuration files)
localCluster/etc/init.d (the folder with the init scripts for each one of the parts)
localCluster/var (the folder for logs and pid files)

5) Run the script with the 'config' command to configure each part of the cluster (it will start each one, configure it and then stop them):
./cluster_in_a_box_mongodb.py config

6) Run the script with the 'start' command to start each and every instance of the cluster:
./cluster_in_a_box_mongodb.py start
You should now have all the nodes, config mongod, replicasets and mongos instance, up and running.
You should be able to check the logs of all the instances under localCluster/var

You can check the status of all the parts (run serverStatus on each instance and display the results on the console) by running:
./cluster_in_a_box_mongodb.py status

If you want to stop them gracefully, you can run:
./cluster_in_a_box_mongodb.py stop

You can also 'kill' them (simulating a server crash) with 'kill'
./cluster_in_a_box_mongodb.py kill

You can also operate on individual parts of the cluster with the init scripts that you are going to find under localCluster/etc/init.d.
For instance, you will be able to stop/kill the secondary node of the replicaset supporting the first shard of the cluster with the script:
localCluster/etc/init.d/localCluster.shard.1.rs.2.mongod [start|stop|kill]

This is still a pre-alpha version, so there are a ton of things that I still need to implement/correct.

Let me know if you have any questions regarding the tool, how to use it or if/when if fails (like I said, it's going to fail a lot at this stage).

Running the script without any options, like in ./cluster_in_a_box_mongodb.py will show the several options that you can use.
Some of them work, some don't (yet). If in doubt, check the source code.... or ask me ;-)


An example of the files and folders that it will create:

./etc/localCluster.cfg.1.conf
./etc/localCluster.cfg.2.conf
./etc/localCluster.cfg.3.conf
./etc/localCluster.mongos.conf
./etc/localCluster.shard.1.rs.1.conf
./etc/localCluster.shard.1.rs.2.conf
./etc/localCluster.shard.1.rs.3.conf
./etc/localCluster.shard.2.rs.1.conf
./etc/localCluster.shard.2.rs.2.conf
./etc/localCluster.shard.2.rs.3.conf
./etc/init.d/localCluster.cfg.1.mongod
./etc/init.d/localCluster.cfg.1.mongod.safe
./etc/init.d/localCluster.cfg.2.mongod
./etc/init.d/localCluster.cfg.2.mongod.safe
./etc/init.d/localCluster.cfg.3.mongod
./etc/init.d/localCluster.cfg.3.mongod.safe
./etc/init.d/localCluster.mongos.mongod
./etc/init.d/localCluster.mongos.mongod.safe
./etc/init.d/localCluster.shard.1.rs.1.mongod
./etc/init.d/localCluster.shard.1.rs.1.mongod.safe
./etc/init.d/localCluster.shard.1.rs.2.mongod
./etc/init.d/localCluster.shard.1.rs.2.mongod.safe
./etc/init.d/localCluster.shard.1.rs.3.mongod
./etc/init.d/localCluster.shard.1.rs.3.mongod.safe
./etc/init.d/localCluster.shard.2.rs.1.mongod
./etc/init.d/localCluster.shard.2.rs.1.mongod.safe
./etc/init.d/localCluster.shard.2.rs.2.mongod
./etc/init.d/localCluster.shard.2.rs.2.mongod.safe
./etc/init.d/localCluster.shard.2.rs.3.mongod
./etc/init.d/localCluster.shard.2.rs.3.mongod.safe
./var/localCluster.cfg.1.log
./var/localCluster.cfg.1.log.cfg_run
./var/localCluster.cfg.2.log
./var/localCluster.cfg.2.log.cfg_run
./var/localCluster.cfg.3.log
./var/localCluster.cfg.3.log.cfg_run
./var/localCluster.mongos.log
./var/localCluster.mongos.log.cfg_run
./var/localCluster.mongos.pid
./var/localCluster.shard.1.rs.1.log
./var/localCluster.shard.1.rs.1.log.cfg_run
./var/localCluster.shard.1.rs.2.log
./var/localCluster.shard.1.rs.2.log.cfg_run
./var/localCluster.shard.1.rs.3.log
./var/localCluster.shard.1.rs.3.log.cfg_run
./var/localCluster.shard.2.rs.1.log
./var/localCluster.shard.2.rs.1.log.cfg_run
./var/localCluster.shard.2.rs.2.log
./var/localCluster.shard.2.rs.2.log.cfg_run
./var/localCluster.shard.2.rs.3.log
./var/localCluster.shard.2.rs.3.log.cfg_run


