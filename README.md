# mysql-master-replication-nodes
Docker-compose example for mysql "master-slave" replication
## Usage
Start the daemonized stack with:

    docker-compose up -d

A different port is mapped locally for each node, we will have the following TCP ports available on our Docker Host:

master: 3301, slave-1: 3302, slave-2: 3303, slave-3: 3304
## Files guide
- conf / initdb.d: Contains sql scripts that will be executed in mysql when lifting containers. In docker-compose.yml it is defined as we introduce it to the different containers.
    
- dbStocks.sql: Sample database loaded at master startup. Will be automatically replicated to the other nodes with mysql replication process.

- node_master.sql:
    We give slave replication privileges to all tables in all schemas. In this case with the root user although a user can be created specifically for it.
    The node-slave-x hosts are the names of the hosts on the internal bridge network defined in docker-compose.yml. It automatically resolves the correct IP.

- node_slaves.sql: These are the replication instructions for the replicated nodes (change master) that will be loaded after starting the container.

    We indicate the master host, the mirror user, the password, the mirror binary log file, and the position (4 is the first and will load all the changes including the dbStocks database)

- conf / my.cnf.d: Contains the my.cnf files for each node

- node_master_my.cnf:
    We indicate the server ID and the name of the log for master
    
- node_slave_x.cnf:
    We indicate the server ID for the slaves

- docker.cnf: File with custom configuration of the mysql image, it is used in this case to allow the resolution of hosts by name.
    
- .env: File with environment variables. In this case it contains only the root password for all databases.
    
- docker-compose.yml: File with the docker-compose instructions to set up the nodes in Docker.
