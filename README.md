# docker-byteball-witness
Docker image for byteball-witness

## Usage

The simplest way to create the docker image and run byteball-witness is below.

```console
$ docker build -t byteball-witness .
$ docker run -it byteball-witness
```

although it is always a good idea to give a container a name you can remember:

```console
$ docker run -it --name my-byteball-witness byteball-witness
```

Running the container at the first time will set up some configuration items
such as the name of the device so it is important to run the container in an
interactive mode. When finished, release the container by pressing Ctrl-p Ctrl-q.
That will detach the console with leaving the container running.

To stop the container and then restart it again, use:

```console
$ docker stop my-byteball-witness
$ docker start -i my-byteball-witness
```

Again, the container has to be started in interactive mode because the app asks
for a passphrase.

### Using volumes

Although the byteball-witness docker image has been set up to create a volume
and store the byteball runtime files on the host filesystem, using a named volume
is recommended so containers can be dropped and recreated easily by referencing
the existing storage by a simple name:

```console
$ docker volume create --name byteball-witness
$ docker run -it --name my-byteball-witness -v byteball-witness:/byteball byteball-witness
```

NOTE: The configuration files are stored in the `/byteball` folder inside the container. 

### Changing the configuration

In order to change the configuration file, stop the byteball-witness container
start a new one like below:

```console
$ docker run -it --rm -v byteball-witness:/byteball byteball-witness vi /byteball/conf.json
```

This will mount the named byteball volume and open the conf.json file in the
`vi` text editor. When you quite from `vi` the container will automatically
delete itself due to the `--rm` flag.

Now you can start the container again and the app will start up with the 
changed configuration.

A sample configuration:

```
{
        "deviceName": "witness",
        "admin_email": "admin@example.com",
        "from_email": "fromemail@example.com",
        "permanent_paring_secret": "randomstring",
        "control_addresses": ["DEVICE ALLOWED TO CHAT"],
        "payout_address": "WHERE THE MONEY CAN BE SENT TO"
}
```

See configuration options here:
* [byteball/byteballcore](https://github.com/byteball/byteballcore)
* [byteball/byteball-witness](https://github.com/byteball/byteball-witness)

### Checking the log file

In case you need to check the log files you can use the following command:

```console
$ docker run -it --rm -v byteball-witness:/byteball byteball-witness less /byteball/log.txt
```

### Exposing port to the host system

If you enabled the default websocket port (6611) you may want to map it a port
on your host system. You have to create the container as below, but you may
first want to stop and remove the running container before creating a new one.

```console
$ docker stop my-byteball-witness
$ docker rm my-byteball-witness
$ docker run -it --name my-byteball-witness -v byteball-witness:/byteball -p 6611:6611 byteball-witness
```

This will map the 6611 port of the host system to the 6611 port of the container.

