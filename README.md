# Docker build for Oracle Database 18c Express Edition (XE)

## Prerequisites

1. [Download](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html) the RPM from Oracle Technology Network and save to folder. 

## Build Image

```bash
-- Clone repo
git clone git@github.com:fuzziebrain/docker-odb18c-xe.git

-- Set the working directory to the project folder
cd docker-odb18c-xe

-- Copy the RPM to docker-odb18c-xe/files
cp ~/Downloads/oracle-database-xe-18c-1.0-1.x86_64.rpm files/

-- Build Image
docker build -t oracle-xe:18c .
```

## Run Container

_Note it will take a while on first run for the `oracle-xe configure` to complete_

`/tmp/apex` is an optional mapping to install APEX
__
```bash
docker run -d \
  --name=oracle-xe \
  -p 32118:1521 \
  -p 35518:5500 \
  --volume ~/docker/oracle-xe:/opt/oracle/oradata \
  --volume ~/docker/apex:/tmp/apex \
  oracle-xe:18c
```

## Connecting

### SQL

```bash
-- Connect to CDB
sqlcl sys/Oracle18@localhost:32118/XE as sysdba


-- Connect to default PDB
sqlcl sys/Oracle18@localhost:32118/XEPDB1 as sysdba

```

### Bash

In some cases you may need to login to the server to modify or test something on the file system.
```bash
docker exec -it oracle-xe bash -c "source /home/oracle/.bashrc; bash"
```

### OEM

_Note: Flash is required_</br>

https://localhost:35518/em


## Other

Install `emp` and `dept` sample tables:</br>

`@https://raw.githubusercontent.com/OraOpenSource/OXAR/master/oracle/emp_dept.sql`