## Docker Exec: Sane Docker Debugging without SSH

The new Docker Exec command (available in Docker version 1.3+) allows users to run any command in any running container. These commands can either run in the background, or in the foreground. The canonical example given is to run bash in a container to do debugging. For example, in Airstack Composer, I am building a container that has a run command like this:

```bash
docker run -p 127.0.0.1:2200:22 -p 80:80 \
  --name=my-webapp --workdir /home/airstack \
  -e HOME=airstack mydevelopment/my-webapp:staging \
  /etc/runit/2 syslog
```

As you'll notice, I have runit running within this container to start up the multiple services that this application needs. And I've included a tiny ssh server (dropbear in this example) to allow administrators and users to have an easy way to inspect the container as needed during runtime.

The new `docker exec` command would allow me to run the container like this:

```bash
docker run -p 80:80 \
  --name=my-webapp --workdir /home/airstack \
  -e HOME=airstack mydevelopment/my-webapp:staging \
  /etc/runit/2 syslog
```
Notice that I no longer have the need to open up port 22 even locally for ssh. +1 for security. 
Removing SSH also significantly reduces the complexity of having to manage keys and/or passwords. +1 for peace of mind.

So how to debug in this brave new SSH-less world?

First, a quick helper alias to allow us to quickly find the id of a running container:
```bash
alias docker-id='docker inspect --format="{{ .Id }}"'
```
And then when I want to debug the application, I can simply run:
```bash
docker exec -it $(docker-id my-webapp) /bin/bash
```

And I'm in my container, able to debug as expected. 

The downside here is that users can now mess up running containers like never before... there doesn't seem to be any way to turn off or limit the `docker exec` functionality. And `docker exec` commands aren't currently logged like sudo logs. I've opened up a ticket with docker to add command logging options to `docker exec`: https://github.com/docker/docker/issues/8662 And an additional issue for adding the ability to disable `docker exec` if desired: https://github.com/docker/docker/issues/8664

I'm planning to do additional testing to see if docker exec opens up other possibilities for streamlining our workflow and reducing the complexity of our running containers.
