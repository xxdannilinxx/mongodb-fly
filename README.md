![Mongodb Docker Compass](https://raw.githubusercontent.com/bogordesaincom/mongodb-fly/master/images/mongodbcompass.png)

### Install Mongodb with Debian

Allow Ip4 and allow IP6

Create app

```
fly launch
```

Create Volume Storage :

```
fly volumes create gamazap_database_storage --size 4 --region "gru"
```

Activate IP6

```
fly ips allocate-v6 --private --app gamazap-db
```

IP4

```
fly ips allocate-v4 --app gamazap-db
```

### Login in to SSH For Secure Mongodb Access

```
flyctl ssh console --app gamazap-db
```

Create User and Password in `admin` db

Change `gamazap` and `changemepassword`

```
mongosh

test > use admin

admin > db.createUser({ user: "gamazap", pwd: "changemepassword", roles: [{ role: "userAdminAnyDatabase", db: "admin"}, "readWriteAnyDatabase" ]})

```

Change Docker file

```
RUN sed -i "s,\\(^[[:blank:]]*bindIp:\\) .*,\\1 0.0.0.0," /etc/mongod.conf
```

Change to

```
COPY mongod.conf /etc/mongod.conf
```

Deploy Again to Fly.io

```
fly deploy --config=fly.toml --no-cache
```

Change memory size:

```
fly scale vm shared-cpu-2x
fly machine update 3287469ec35d58 --memory 1024
```

### Example Login using mongodb Compass :

```
mongodb://gamazap:changemepassword@gamazap-db.fly.dev:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.6.1
```
