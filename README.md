# Open mHealth Capstone Control Script

### Overview

Provides an easy way to manage control of the debian-based server (Ubuntu 14.04 or Debian Jessie and higher can be used). This includes installation, updates, backups, starting, and stopping.
Commands can easily be added to crontab for automation.

Detailed instructions for setting up the bare-bone storage endpoint server without the use of a script can be found [here](https://github.com/openmhealth/omh-dsu-ri).

### Installation

The omh-capstone service must be added to /etc/init.d/ to work properly.

To install this script

1. Clone this repository
1. Move omh-capstone to the /etc/init.d/ directory
1. Make the file executable by running `sudo chmod +x /etc/init.d/omh-capstone`

Commands are used by making a call to the service using `sudo service omh-capstone {command}`

After using the install command, you will have to find the container that postgresql is running in.

1. `screen -x omh-capstone`
1. `docker ps`

Find the container ID for the postgresql container, then connect to it using `docker exec -it {ID} bash`

Once connected, issue the following commands:

1. `su postgres`
1. `psql`
1. `\c omh`
  * Add a row to the oauth_client_details table, as shown in [this script](https://github.com/openmhealth/omh-dsu-ri/blob/master/resources/rdbms/common/oauth2-sample-data.sql).

Once finished, exit back to the main screen

1. `\q`
1. `exit`
1. `exit`
1. Exit the screen by pressing Ctrl+D

In order to allow remote systems and users to connect to the server, you will need to create a macvlan network for docker and connect the authorization and resource servers to it.

### Postman

A postman collection is included to help with starting up. If an end user has not been created yet, you will have to create one using

`curl -H "Content-Type:application/json" --data '{"username": "testUser", "password": "testUserPassword"}' http://host:8082/users`

