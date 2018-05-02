# Open mHealth Capstone Control Script

### Overview

Provides an easy way to manage control of the server. This includes installation, updates, backups, starting, and stopping.
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
1. `\q`
1. `exit`
1. `exit`

Add an end user by issuing this command, replacing host with the docker host (`docker-machine ip`):

1. `curl -H "Content-Type:application/json" --data '{"username": "testUser", "password": "testUserPassword"}' http://host:8082/users`

In order to allow remote systems and users to connect to the server, you will have to change the virtual network adaptor.

1. Open Virtualbox Manager
1. Open the settings for the virtual machine the server uses
1. In the Network tab, select Adaptor 2 and change "Attached to: Host-only Adapter" to your network card
