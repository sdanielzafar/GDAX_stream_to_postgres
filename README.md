# GDAX to remote MongoDB
The goal of this repository is to demonstrate how to stream data from one EC2
instance to a remote MongoDB instance on another EC2. Files pertaining to a
hosted MongoDB instance will be in the `mongo_server` directory. Files
pertaining to streaming from GDAX will be located in the `gdax_server` directory.

Users should secure-copy relevant files to the correct EC2 instance with:
`scp -i my_AWS_PEM.pem mongo_server/ ubuntu@ec2-xx-my-mongo-server-xx.compute-1.amazonaws.com`
OR
`scp -i my_AWS_PEM.pem gdax_server/ ubuntu@ec2-xx-my-GDAX-server-xx.compute-1.amazonaws.com`.


### Mongo server setup
The first goal is to get Mongo up and running on a Ubuntu 16.04 Amazon EC2
instance. Begin by ssh'ing into your EC2:

1. `ssh -i my_AWS_PEM.pem ubuntu@ec2-xx-my-mongo-server-xx.compute-1.amazonaws.com`

2. Run `mongo_setup.sh` to get started. This should take care of most of the
installation work.
3. Make sure Mongo has read & write permissions on the `/data/db` directory.
4. Create a new username and password for MongoDB by using the following:

  ```
  use my_db

  db.createUser({
      user: 'my_username',
      pwd: 'my_password',
      roles: [{ role: 'readWrite', db:'my_db'}]
  })
  ```
5. Adjust the `/etc/mongo.conf` file. This file is a YAML file, despite the
extension.
  - First, change the `bindIp` parameter to a public-facing IP.
  - Second, make sure the `security` parameter is not commented out. Add
  `authorization: 'enabled'` under the `security` parameter.
6. Run `sudo service mongod start` to start the instance.

Note: run `mongo db -u "my_username" -p "my_password"` to take a peek inside
your running MongoDB instance.

### GDAX streaming client setup
Next, let's use a slightly altered version of the Python GDAX client's
WebsocketClient so we can stream events into the remote MongoDB.

Begin by ssh'ing into your EC2:

1. `ssh -i my_AWS_PEM.pem ubuntu@ec2-xx-my-GDAX-server-xx.compute-1.amazonaws.com`
2. Adjust `properties.json` with the correct information.
2. Run `GDAX_setup.sh` to get started. This should take care of most of the
installation work and start the streaming process!
