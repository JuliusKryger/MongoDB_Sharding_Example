# MongoDB Sharding

1. [Opret Config Server Replica Set](#opret-config-server-replica-set)
2. [Opret Shard Replica Sets](#opret-shard-replica-sets)
3. [Start en mongos for det Sharded Cluster](#start-en-mongos-for-det-sharded-cluster)
4. [Forbind til det Sharded Cluster](#forbind-til-det-sharded-cluster)
5. [Tilføj Shards til Clustret](#tilføj-shards-til-clustret)

## Opret Config Server Replica Set

### 1. Start hver medlem af config server replica set

Eksempel:

    mongod --configsvr --replSet configrepl --dbpath ./data --bind_ip localhost --port 27017

### 2. Forbind til en af config serverne

Eksempel:

    mongosh --host localhost --port 27017

### 3. Initier replica set

Eksempel:

    rs.initiate()

## Opret Shard Replica Sets

### 1. Start hver medlem af shard replica set

Eksempler:

mongo1:

    mongod --shardsvr --replSet rs1 --dbpath ./data --bind_ip localhost --port 27020

mongo2:

    mongod --shardsvr --replSet rs1 --dbpath ./data --bind_ip localhost --port 27021

mongo3:

    mongod --shardsvr --replSet rs1 --dbpath ./data --bind_ip localhost --port 27022

### 2. Forbind til et medlem af shard replica set

Eksempel:

    mongosh --host localhost --port 27020

### 3. Initier replica set

Eksempel:

    rs.initiate(
      {
        _id : "rs1",
        members: [
          { _id : 0, host : "localhost:27020" },
          { _id : 1, host : "localhost:27021" },
          { _id : 2, host : "localhost:27022" }
        ]
      }
    )

## Start en mongos for det Sharded Cluster

Eksempel:

    mongos --configdb rsconf/localhost:27017 --bind_ip localhost --port 27019

## Forbind til det Sharded Cluster

Eksempel:

    mongosh --host localhost --port 27019

## Tilføj Shards til Clustret

Forbind til mongos og tilføj shards til clustret ved hjælp af `sh.addShard()` metoden.

Eksempel:

    sh.addShard("rs1/localhost:27020,localhost:27021,localhost:27022")
