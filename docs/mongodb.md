# Install and configure mongodb

## Install mongodb:
[Install on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

## Enable auth and open MongoDB access up to all IPs
```
> sudo vim /etc/mongod.conf
```

```CMake
# network interfaces
net:
  port: 27017
#  bindIp: 127.0.0.1  <- comment out this line
```

## Create user admin
Connect to mongodb and:
```CMake
> use admin
> db.createUser(
  {
    user: "admin",
    pwd: "admin123",
    roles: [ 
      { role: "userAdminAnyDatabase", db: "admin" }
    ]
  }
)
```

## Change password
```CMake
> use admin
> db.changeUserPassword("admin", "admin123456789")
```

## Modify the user’s access.
### Grant a role 
```CMake
> use admin
> db.grantRolesToUser("admin",
    [{ role: "dbAdminAnyDatabase", db: "admin" }]
)
```
### Revoke a role
```CMake
> use admin
> db.grantRolesToUser("admin",
    [{ role: "userAdminAnyDatabase", db: "admin" }]
  }
)
```

## To authenticate after connecting
```CMake
> use admin
> db.auth("admin", "admin123" )
```

## Create user to read and write
Connect to mongodb and:
```
> use test
> db.createUser(
  {
    user: "userTest",
    pwd: "test123",
    roles: [ 
      { role: "readWrite", db: "test" } 
    ]
  }
)
```

## Security Authorization
```
> sudo vim /etc/mongod.conf
```
```CMake
#processManagement:

security:
  authorization: "enabled"
```
```
> sudo service mongod restart
> mongo -u admin -p "admin123" --authenticationDatabase "admin"
```

## Change stores data files
```
> sudo vim /etc/mongod.conf
```
Current
```CMake
# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
```
Change to:
```CMake
# Where and how to store data.
storage:
  dbPath: /mnt/mongodb/data
```
Current
```CMake
# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
```
Change to:
```CMake
# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /mnt/mongodb/log/mongod.log
```

## Set permissions
```
> sudo chown -R mongodb:mongodb /mnt/mongodb/
```

## Start MongoDB
```
> sudo service mongod start
```

## Stop MongoDB
```
> sudo service mongod stop
```

## Restart MongoDB
```
> sudo service mongod restart
```

## Status MongoDB
```
> sudo service mongod status
```