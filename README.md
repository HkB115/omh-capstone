# omh-capstone
Provides an easy way to manage control of the server. This includes installation, updates, backups, starting, and stopping.
Commands can easily be added to crontab for automation

The omh-capstone service must be added to /etc/init.d/ to work properly.
To install:
  Clone this repository
  Move omh-capstone to the /etc/init.d/ directory
  Make the file executable by running "sudo chmod +x /etc/init.d/omh-capstone"

Controls are used by making a call to the service using "sudo service omh-capstone {command}"

Detailed instructions for setting up the bare-bone storage endpoint server without the use of a script can be found at https://github.com/openmhealth/omh-dsu-ri

After using the install command, you will have to find the container that postgresql is running in. This can be done by running "docker ps"
Run the following commands:
  "screen -x omh-capstone"
  "docker ps"
Find the container ID for the postgresql container, then connect to it
Example: "docker exec -it {ID} bash"
Issue the following commands:
  "su postgres"
  "psql"
  "\c omh"
Add a row to the oauth_client_details table, as shown in this script https://github.com/openmhealth/omh-dsu-ri/blob/master/resources/rdbms/common/oauth2-sample-data.sql
  "\q"
  "exit"
  "exit"
Add an end user by issuing this command, replacing host with the docker host (docker-machine ip):
  "curl -H "Content-Type:application/json" --data '{"username": "testUser", "password": "testUserPassword"}' http://host:8082/users"

In order to allow remote systems and users to connect to the server, you will have to change the virtual network adaptor.
To do so:
  Open Virtualbox Manager
  Open the settings for the virtual machine the server uses
  In the Network tab, select Adaptor 2 and change "Attached to: Host-only Adapter" to your network card

