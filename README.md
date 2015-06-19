Quagga in Docker
-----------------

To use, you must run a privileged container.

For instance, to run a bare, unconfigured Quagga and
spawn a vtysh shell, one may use:

```
docker run --privileged --rm -it -n quagga ewindisch/quagga
```

The user may disconnect from this using Ctrl-P, Ctrl-Q and
reattach to vtysh using 'docker attach -it quagga'.

Mind the above is unconfigured. Generally, users will wish
to create or load to load their own configurations.

For managing configuraiton, it is recommended
for users to do one of the following:

Option #1: Use Docker volumes
-----------------------------

Load your configuration into a configuration directory and
provide it as a bind-mount into Quagga. The following example
checks out our Git repository and uses the configuration there.

```
git clone https://github.com/ewindisch/docker-quagga
ed docker-quagga/config/zebra.conf
docker run --privileged --rm -d -v $PWD/config:/etc/quagga ewindisch/quagga
```

Using volumes is recommended if seeking to run a separate container
per quagga daemon.

Option #2: Bake-in configuration
--------------------------------

One may base an image off of this, loading local configuration.

The following example checks out our github repo and creates
a new image containing local configuration changes.

```
git clone https://github.com/ewindisch/docker-quagga
ed docker-quagga/config/zebra.conf
docker build -t my-quagga <<<"FROM ewindisch/quagga"
docker run --privileged --rm -d my-quagga
```

Option #3: Be a pet
-------------------

Finally, one may simply run this image without '--rm',
tweak the configuration, and then using 'docker stop', 'docker start',
etc. This way is more traditional for management, but is not the
recommended solution with Docker.
