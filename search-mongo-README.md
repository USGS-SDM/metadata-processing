# Creating a Realtime Database Connection to a Search Index (prototype)

### Using MongoDB and Elasticsearch to store and index metadata files

There's community driven mongodb connectors (https://github.com/yougov/mongo-connector) that interface with search indexes in realtime using replication. This workflow shows how we could intake JSON metadata files as mdjson, store them in the database and build a search index based from it. 

__mongo connector__ 

​	https://github.com/yougov/mongo-connector

__Elasticsearch__

​	Due to version conflicts I found an older combination that works well (v2.4)

__Mongodb__

​	Due to version conflicts (v3.2)

## Installation

For testing you can download older released via the official websites or if you are on OSX brew might be the fastest. 

```
# es
brew install elasticsearch@2.4

# mongo
brew install mongodb@3.2
```

The connector is a python package so please install other required packages to run (I used a virtualenv python3.6)

```
pip install pymongo==3.7.2 
pip install elastic2-doc-manager==1.0.0     
```

## Running

1. Make sure mongodb is setup to use 

```
# create a data folder your user has access to
$ mkdir /data/db

# create a database 
$ mongo

> use sdc
> quit()

# load a few records
$ mongoimport -d sdc data/Test_deterrence_of_bighead_carp_Hypophthalmichthys_nobilis_to_a_broadband_sound_stimulus.mdjson
```

2. Run as a development replication

```
mongod --replSet development

# open mongo
mongo
rs.initiate();
```

3. Run mongo connector

```
mongo-connector -m localhost:21017 -t localhost:9200 -d elastic2_doc_manager
```

4. Test elastic

```
$ curl -XGET 'http://localhost:9200/sdc/_mapping/?pretty'
```

### Helpful Resources

http://eloquentdeveloper.com/2016/09/12/integrate-elasticsearch-mongodb/

https://medium.com/@xoor/indexing-mongodb-with-elasticsearch-2c428b676343

## Provisional Software Disclaimer

Under USGS Software Release Policy, the software codes here are considered preliminary, not released officially, and posted to this repo for informal sharing among colleagues.

This software is preliminary or provisional and is subject to revision. It is being provided to meet the need for timely best science. The software has not received final approval by the U.S. Geological Survey (USGS). No warranty, expressed or implied, is made by the USGS or the U.S. Government as to the functionality of the software and related material nor shall the fact of release constitute any such warranty. The software is provided on the condition that neither the USGS nor the U.S. Government shall be held liable for any damages resulting from the authorized or unauthorized use of the software.