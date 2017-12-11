# Stream GDAX to postgres
The goal of this repository is to stream data from one GDAX websocket API to 
postgres. These servers are physical, not E3 on AWS. Both are running 
Ubuntu 16.04.3 LTS. This was forked from 
a demo project going to a remote MongoDB ofrom AWS E3.

Files pertaining to postgres instance will be in the `postgres_server`
directory. Files pertaining to streaming from GDAX will be located in the
`gdax_server` directory. Setup details for the server may be included.
Don't forget to change your inbound security rules for the Mongo
instance!

### Postgres server setup
The first goal is to get Postgres up and running on a Ubuntu 16.04 Server. 
Notes on how Postgres was set up for streaming will be included here:

1. Make sure postgres is installed.
2. Open Postgres for remote connection on port 5432. Configure 
   postgresql.conf to do so.

### GDAX streaming client setup
Next, let's use a slightly altered version of the Python GDAX client's
WebsocketClient so we can stream events into Postgres. Notes on how
the client was set up and executed wil be included here.
