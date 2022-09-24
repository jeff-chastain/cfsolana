# CF Solana API

This is the development repo for [Solana API](https://docs.solana.com/developing/clients/jsonrpc-api) integrated with [Adobe ColdFusion](https://coldfusion.adobe.com/).

## Running Locally

### Prerequisites

1. [Docker](https://www.docker.com/) or other container virtualization software.
2. [Postman](https://www.postman.com/) - We will use this to test the API's. Download and import our specific [CF Solana Snapshot](https://www.getpostman.com/collections/393462fe546943d1a8c0).
3. A Solana account is needed to call the Solana API. Download a Solana wallet such as [Phantom](https://phantom.app/download) to create a new account and receive a wallet address.

### Getting Started

0. Copy `.env.sample` to `bin/.env`
1. Clone this repository down to your computer.
2. `chmod +x install.sh` (this just makes it executable)
3. Make sure docker or other container software is running.
4. `./install.sh`
5. Wait a few minutes. The install script is running the full docker setup and the Lucee server can take a few minutes to fully initialize.
6. Open web browser to `localhost:8080`
7. Follow through the ContentBox wizard to setup the first user of the CMS.
8. Use a tool like Postman to query the APIs.
9. Note: for most API calls you will need to authorize through the User login route, using the username and password signed up with on the ContentBox wizard.

**NOTE**:

To support Apple M1 chips, we are adding `platform: linux/amd64` to the docker compose for each service.

## Troubleshooting

**Question**: How do I do local development?

**Answer**: We are mounting the necessary files needed for development in to the docker container via volume mount points. You can see specifically what we are mounting in `bin/docker-compose.yml` under the `cf-solana` service's `volumes` section. When this container is running, the files mounted from the local file system are automatically synced to the docker container file system. These changes are synced both ways, so a change made on the local filesystem will update in docker and updates in docker will update on local filesystem. *This will only happen for the files mounted*.

**Question**: Where are the database and container environment variable configurations?

**Answer**: The MySQL environment variables are set in `bin/.env` that are then used in the `bin/docker-compose.yml` to be passed in to the docker containers for initialization.

**Question**: How do I manually restart the ColdBox server from within the docker container?

**Answer**: Navigate to the installation directory, in our case that is `/app`. From there, run `box` to initialize the ColdBox REPL. Then, once it is loaded you should see the commandbox the welcome message. Run `server stop` to stop the server. You can verify what the status of the server is by running `server list`. Then, to restart the server run `server start host=0.0.0.0 port=8080 openbrowser=false verbose=true`. It is important that the server is run on `0.0.0.0` so that it is accessible to outside of the container without using something like NGINX. 

**Question**: How do I enter a docker container from the command line?

**Answer**: First will need to list out all the running docker containers with `docker ps`. Find the container that want to enter and look for the name under the `NAMES` column. Then, run `docker exec -it <container_name> /bin/bash`. You will be moved into the docker container and can run commands from that system.

**Question**: How do I remove the development docker setup once I am done?

**Answer**: We included a tear down script `cleanup.sh` in the `bin` folder that will tear down everything, including the docker volumes. You can also manually stop the containers by name through Docker.

**Question**: How do I send emails through SMTP on development server?

**Answer**: when running ContentBox in development mode, emails are never sent via SMTP.  They are always logged to the file system. You can find the email logs in the ContentBox installation folder under /config/logs/mail/. When run in production mode, the admin SMTP settings are honored and if not specified, they default back to the CF Admin mail settings.

**Question**: How do I send emails through SMTP on staging or production?

**Answer**: Yes, in production mode ContentBox will send via SMTP and can use any SMTP server that is reachable (i.e. network security, etc.)

**Question**: I am getting a message about aardvark-dns not being available. What is this?

**Answer**: aardvark-dns is Podman's new container DNS resolver; it is used to communicate with containers on the same network. Consequently, you will need to install it on the host machine with your operating system's package manager.

**Question**: I am getting a database connection issue with Podman the first time I start the containers.

**Answer**: This is a known problem that I have been encountering also; I think it is a startup lag where things are still coming up. Simply waiting it out and refreshing the page from time to time fixes this. If there is a real problem and it does not come up, please do follow an issue. This problem also manifests itself with a browser message like "connection was reset" or some similar verbiage.
