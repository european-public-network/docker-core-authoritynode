# evan.network Signer node

The evan.network signer nodes, is a node that is allowd to sign new blocks in the evan.network. This node retrieves sent transactions to the network and signs them into new blocks. To guarantee security, this node has no opened ports to any RPC or Websocket api. It only signs transactions and seal blocks.

## Requirements

The technical requirements to the installed server are :

AWS:
 - T2.xlarge /T2.large Instance
 - Min. 300GB EBS storage

Azure:
 - Standard_D4_v3 / Standard_D2_v3
 - Min 300GB Premium storage

DigitalOcean:
- Standard droplet with
- 8GB RAM, 4 vCPU, 160gb SSD

OnPremise:
 - 2 Xeon CPU's
 - 16GB RAM
 - 300GB SSD storage

Open Ports:
30303 (TCP & UDP) – Blockchain Sync

- 1 fixed IP address

To start the Authoritynode you must have installed [docker](https://www.docker.com/get-docker) and [docker-compose](https://docs.docker.com/compose/install/) on your Server

## Configuration

### Create new Sigining Account

Before you can start the authoritynode, you have to create the signing account.

To Create a new account run the following command in the core-authoritynode folder:

`sudo docker-compose run parity account new --config /parity/config/parity.toml`

After this command, you will be prompted to create a password for your new account. Please use a safe password and store it on a safe location:


>Loading config file from /parity/config/parity.toml
>
>Please note that password is NOT RECOVERABLE.
>
>Type password:
>
>Repeat password:


When you have created a password your accountid will be logged on the console like:

`0x0XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`

Send this accountId back to the evan.network team to allow your accountId as a new signing node.

### Configure Parity

Edit the following files:

- docker-compose.yml
replace the line `[Authority address]` with your previous created accountid
uncomment the "--chain" and the "volumes" parameter for the used chain

- parity-config/pw.txt
put the password for your account in this file

- ethstats/app.json
replace the placeholder [company name] and [admin email] with your informations
replace the placeholder [ws secret] with the password for the given network
replace the placeholder [status url] with the given network status (wss://status.evan.network or wss://teststatus.evan.network)

## Starting

To start your authoritynode, you have to simply run "docker-compose up -d" in the directory.

## Logging

To access the log file from the authoritynode, you can use the command "docker logs -f --tail 1000 RUNNING_CONTAINER_NAME" to get the last 1000 log lines from the container. The RUNNING_CONTAINER_NAME can be replaced with the following names:

- parity_signer (The blockchain instance)
- ethstats_monitor (Status monitor connection to https://status.evan.network / https://teststatus.evan.network)

## Troubleshooting

If any of the nodes behave wrong, you can restart the container with the command "docker restart RUNNING_CONTAINER_NAME"

Please reach out our support team with the last logs of your container, to check if there's anything wrong.
