<!--

********************************************************************************

WARNING:

    DO NOT EDIT "kaazing-gateway/README.md"

    IT IS AUTO-GENERATED

    (from the other files in "kaazing-gateway/" combined with a set of templates)

********************************************************************************

-->

# Supported tags and respective `Dockerfile` links

-	[`latest`, `5.6.0` (*Dockerfile*)](https://github.com/kaazing/gateway.docker/blob/a40c8da9d2c2925bdd78b9a6d1b6da3fe89322d1/Dockerfile)

# Quick reference

-	**Where to get help**:  
	[the Docker Community Forums](https://forums.docker.com/), [the Docker Community Slack](https://blog.docker.com/2016/11/introducing-docker-community-directory-docker-community-slack/), or [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=docker)

-	**Where to file issues**:  
	[https://github.com/kaazing/gateway.docker/issues](https://github.com/kaazing/gateway.docker/issues)

-	**Maintained by**:  
	[the Kaazing Docker Maintainers](https://github.com/kaazing/gateway.docker)

-	**Published image artifact details**:  
	[repo-info repo's `repos/kaazing-gateway/` directory](https://github.com/docker-library/repo-info/blob/master/repos/kaazing-gateway) ([history](https://github.com/docker-library/repo-info/commits/master/repos/kaazing-gateway))  
	(image metadata, transfer size, etc)

-	**Image updates**:  
	[official-images PRs with label `library/kaazing-gateway`](https://github.com/docker-library/official-images/pulls?q=label%3Alibrary%2Fkaazing-gateway)  
	[official-images repo's `library/kaazing-gateway` file](https://github.com/docker-library/official-images/blob/master/library/kaazing-gateway) ([history](https://github.com/docker-library/official-images/commits/master/library/kaazing-gateway))

-	**Source of this description**:  
	[docs repo's `kaazing-gateway/` directory](https://github.com/docker-library/docs/tree/master/kaazing-gateway) ([history](https://github.com/docker-library/docs/commits/master/kaazing-gateway))

-	**Supported Docker versions**:  
	[the latest release](https://github.com/docker/docker-ce/releases/latest) (down to 1.6 on a best-effort basis)

# What is the KAAZING Gateway?

The Kaazing Gateway is a network gateway created to provide a single access point for real-time web based protocols that supports load balancing, clustering, and security management. It is designed to provide scalable and secure bidirectional event-based communication over the web; on every platform, browser, and device.

![logo](https://raw.githubusercontent.com/docker-library/docs/ba0031dd881f3b95d30ebf1d6094e99985d8eb6d/kaazing-gateway/logo.png)

# How to use this image

## Up and Running

By default the gateway runs a WebSocket echo service similar to [websocket.org](https://www.websocket.org/echo.html).

You must give your gateway container a hostname. To do this, use the `docker run -h somehostname` option, along with the -e option to define an environment variable, GATEWAY_OPTS, to pass this hostname to the gateway configuration (your hostname may vary):

```console
$ docker run --name some-kaazing-gateway -h somehostname -e GATEWAY_OPTS="-Dgateway.hostname=somehostname -Xmx512m -Djava.security.egd=file:/dev/urandom"-d -p 8000:8000 kaazing-gateway
```

Note: the additional GATEWAY_OPTS options, `-Xmx512m -Djava.security.egd=file:/dev/urandom`, are added in order to preserve these values from the original Dockerfile for the gateway. The `-Xmx512m` value specifies a minimum Java heap size of 512 MB, and `-Djava.security.egd=file:/dev/urandom` is to facilitate faster startup on VMs. See the `Dockerfile` link referenced above for details.

You should then be able to connect to ws://somehostname:8000 from the [WebSocket echo test](https://www.websocket.org/echo.html).

Note: all of the above assumes that `somehostname` is resolvable from your browser. You may need to add an etc/hosts entry for `somehostname` on your `dockerhost ip`.

## Custom Configuration

To launch a container with a specific configuration you can do the following:

```console
$ docker run --name some-kaazing-gateway -h somehostname -e GATEWAY_OPTS="-Dgateway.hostname=somehostname -Xmx512m -Djava.security.egd=file:/dev/urandom" -v /some/gateway-config.xml:/kaazing-gateway/conf/gateway-config.xml:ro -d kaazing-gateway
```

For information on the syntax of the Kaazing Gateway configuration files, see [the official documentation](https://kaazing.com/doc/5.0/index.html) (specifically the *For Administrators* section).

If you wish to adapt the default Gateway configuration file, you can use a command such as the following to copy the file from a running Kaazing Gateway container:

```console
$ docker cp some-kaazing:/kaazing-gateway/conf/gateway-config-minimal.xml /some/gateway-config.xml
```

As above, this can also be accomplished more cleanly using a simple `Dockerfile`:

```dockerfile
FROM kaazing-gateway
COPY gateway-config.xml conf/gateway-config.xml
```

Then, build with `docker build -t some-custom-kaazing-gateway .` and run:

```console
$ docker run --name some-kaazing-gateway -d some-custom-kaazing-gateway
```

## GATEWAY_OPTS

For more information on the GATEWAY_OPTS environment variable, see [Configure Kaazing Gateway Using the GATEWAY_OPTS Environment Variable](https://kaazing.com/doc/5.0/admin-reference/p_configure_gateway_opts/).

# License

View [license information](https://github.com/kaazing/gateway/blob/master/LICENSE.txt) for the software contained in this image.
