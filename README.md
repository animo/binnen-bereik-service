# Dispatch Service

The dispatch service is a server that allows for the creating of emergencies, tracking of users and sending out emergency requests.

## About this repository

This repository contains a Docker Compose file that includes everything you need in order to run the dispatch service. The dispatch service image itself lives on the public Docker registry (see: https://hub.docker.com/r/ssikarim/ufo-dispatch-service).

The Docker Compose file starts two services: a Postgres database and the Dispatch Service. Both of these services are configured using a .env file, which will be described in the following sections.

## Prerequisites
This project relies on two dependencies:
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Running the dispatch service
In order to run the dispatch service, we first need to provide some configuration. This repository contains two environment template files: `database.env.template` and `dispatch.env.template`. What follows is a description of the values in each of these files. Don't forget to remove the `.template` from these files (e.g. rename `dispatch.env.template` to `dispatch.env`).

### Configuration

#### Database.env
- `POSTGRES_USER`
  - The username of the user the Postgres database that will be created belongs to. This can be anything you like.
- `POSTGRES_PASSWORD`
  - The password of the user mentioned above. This can be anything you like.
- `POSTGRES_DB`
  - The name of the database that will be created. This can be anything you like.

#### Dispatch.env
- `UFO_DISPATCH_APN_KEY`
  - This should contain the path to a key file for sending push notifications to iOS devices. Information on how to acquire this file can be found here: https://fluffy.es/p8-push-notification/
  - **NOTE:** Since we've only tested with Android devices, this is not being used. Thus this value may remain empty.
- `UFO_DISPATCH_APN_KEY_ID`
  - This should contain the Apple Push Notification Service user ID for sending push notifications to iOS devices. Information on how to acquire this can be found here: https://fluffy.es/p8-push-notification/
  - **NOTE:** Since we've only tested with Android devices, this is not being used. Thus this value may remain empty.
- `UFO_DISPATCH_APN_TEAM`
  - This should contain the Apple Push Notification Service team ID for sending push notifications to iOS devices. Information on how to acquire this can be found here: https://fluffy.es/p8-push-notification/
  - **NOTE:** Since we've only tested with Android devices, this is not being used. Thus this value may remain empty.
- `UFO_DISPATCH_GCM_ID`
  - This should contain a Google Cloud Messaging ID for sending push notifications to Android devices. Information on how to acquire this can be found here: https://docs.mendix.com/howto/mobile/setting-up-google-firebase-cloud-messaging-server
- `UFO_DISPATCH_AGENT_ENDPOINT`
  - This should contain a publicly available endpoint. This is what mobile devices use to communicate with the server. In order to quickly expose a local endpoint to the internet, we recommend using Ngrok (see: https://ngrok.com/docs)
- `UFO_DISPATCH_INBOUND_TRANSPORT_PORT`
  - This should contain a port number. This is what mobile devices use to communicate with the server.
  - I recommend keeping this set to `80`. However, if you decide to use another value, then be sure to update the value in the `docker-compose` file as well.
- `UFO_DISPATCH_DATABASE_HOST`
  - This should contain the docker-compose service name of the database.
  - I recommend keeping this set to `database`. However, if you decide to use another value, then be sure to update the value in the `docker-compose` file as well.
- `UFO_DISPATCH_DATABASE_PORT`
  - This should contain the port number under which the Postgres database is available.
  - I recommend keeping this set to `5432`. However, if you decide to use another value, then be sure to update the value in the `docker-compose` file as well.
- `UFO_DISPATCH_DATABASE_NAME`
  - This should contain the Postgres database name. It should be set to the same value as you've provided in `database.env`.
- `UFO_DISPATCH_DATABASE_USER`
  - This should contain the Postgres user name. It should be set to the same value as you've provided in `database.env`.
- `UFO_DISPATCH_DATABASE_PASSWORD`
  - This should contain the Postgres user password. It should be set to the same value as you've provided in `database.env`.

### Running the service

After the configuration is complete, you can run the service by executing the following command from the root of this directory:

```
docker-compose up -d
```

In order to verify that the service is running as expected, you can visit http://127.0.0.1:8080/_debug.

### Troubleshooting

If something goes wrong, you can check the server logs by running `docker-compose logs`. If the server has crashed, you can reboot it by running `docker-compose restart`.

In order completely reset the state and clear the database, you can do so by running the following commands:

```
docker-compose down  # stop the services
docker volume prune  # remove all associated volumes
docker-compose up -d # start the services back up
```