Forked from fluentfluentd because fluent-plugin-systemd doesn't play nice with the original version.

Basic changes:
  Remove user fluent from entrypoint.sh - systemd needs to access the system.journal file which is owned by root and the systemd-journal group.
   Build FROM the debian onbuild as systemd doesn't like the armhf build, even though it's being built on a tx2
   
Note this is built on a tx2 (aarch64 arm64v8) using balena (docker but lighter).  We're using fluentd as there isn't a fluent-bit mqtt output plugin (https:/¡/github.com/fluent/fluent-bit/issues/674).  Eventually I'll write our own. The fluent.conf attached is set up to input from systemd and output to stdout.  So to build and run:

git clone https://github.com/winnow-solutions/fluentd-docker-image-arm64.git

cd /mnt/data/fluentd-docker-image-arm64/v1.2/armhf/debian-onbuild

docker build -t test .

docker run -v /run/log/journal/:/run/log/journal -v $PWD/fluent.conf:/fluentd/etc/fluent.conf test:latest

Note:  This is mounting the location of the system.journal (there's also a machine id directory layer in there).  Then mounting the fluent conf in the currrent directory to the default location.

When it runs you should see the fluent initialisation, followed by some logs from the box - something has to be logging to the journal for you to see anything...

### References

[Docker Logging | fluentd.org][5]

[Fluentd logging driver - Docker Docs][6]

## Issues

We can't notice comments in the DockerHub so don't use them for reporting issue
or asking question.

If you have any problems with or questions about this image, please contact us
through a [GitHub issue](https://github.com/fluent/fluentd-docker-image/issues).

[1]: http://alpinelinux.org
[2]: https://hub.docker.com/_/alpine
[3]: https://docs.fluentd.org
[4]: https://www.fluentd.org/plugins
[5]: https://www.fluentd.org/guides/recipes/docker-logging
[6]: https://docs.docker.com/engine/reference/logging/fluentd
[7]: https://hub.docker.com/_/debian
[101]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/alpine/Dockerfile
[102]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/alpine-onbuild/Dockerfile
[105]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/debian/Dockerfile
[106]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/debian-onbuild/Dockerfile
[113]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.2/alpine/Dockerfile
[114]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.2/alpine-onbuild/Dockerfile
[115]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.2/debian/Dockerfile
[116]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.2/debian-onbuild/Dockerfile
