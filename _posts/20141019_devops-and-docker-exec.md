---
layout:     post
title:      Docker Debugging without SSH
date:       2014-11-06 09:00:00
summary:    No more hacks. Finally an official way to debug docker during development or production.
categories: devops docker debugging
---

The new [Docker Exec](https://docs.docker.com/reference/commandline/cli/#exec) command (available in Docker version 1.3+) allows users to run any command in any running container. These commands can either run in the background, or in the foreground. The canonical example given is to run bash in a container to do debugging. 

To give a real example, this week I was refactoring a container and saw that my scripted debug command looked like this:

{% highlight bash %}
docker run -p 127.0.0.1:2200:22 -p 80:80 \
  --name=my-railsapp --workdir /home/swuser \
  -e HOME=swuser mydevelopment/my-railsapp:staging \
  /etc/runit/2 syslog
{% endhighlight %}

As you'll notice, I had [runit](http://smarden.org/runit/) running within this container to start up the multiple services that this particular monolithic application needs (apache/mysql). And I had a tiny ssh server also started (dropbear in this example) to allow administrators and users to have an easy way to inspect the container as needed during runtime.

The new `docker exec` command would allow me to run the container like this:

{% highlight bash %}
docker run -p 80:80 \
  --name=my-railsapp --workdir /home/swuser \
  -e HOME=swuser mydevelopment/my-railsapp:staging \
  /etc/runit/2 syslog
{% endhighlight %}

Notice that I no longer have the need to open up port 22 even locally for ssh. +1 for security. 
Removing SSH also significantly reduces the complexity of having to manage keys and/or passwords. +1 for peace of mind.

##### So how to debug in this brave new SSH-less world?

Run this to quickly debug the last-launched container:

{% highlight bash %}
docker exec -it `docker ps -q -l` /bin/bash
{% endhighlight %}

##### And I'm in my container, able to debug as expected. 

There's another way I use that first requires adding a helper alias to allow us to quickly find the id of a running container:

{% highlight bash %}
alias docker-id='docker inspect --format="{{ .Id }}"'
{% endhighlight %}

With this alias, you can then retrieve the id of a named container:

{% highlight bash %}
docker-id my-railsapp
{% endhighlight %}

To do all at once, I can simply run:

{% highlight bash %}
docker exec -it `docker-id my-railsapp` /bin/bash
{% endhighlight %}

That's all for now. I'm planning to do additional testing to see if `docker exec` opens up other possibilities for streamlining our workflow and reducing the complexity of our running containers. But so far I've integrated it into my workflow and it's been a definite win. Comments / feedback or [corrections via github pull request](https://github.com/VentureUnknown/ventureunknown.github.io/blob/master/_posts/2014-11-06-docker-exec-devops-and-debugging.md) are always welcome.
