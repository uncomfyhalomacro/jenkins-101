# Follow the steps until docker socat

Since podman uses another way for sockets, and it's non-root, you have to enable sockets

```bash
systemctl --user enable --now podman.socket
systemctl --user status podman.socket
```

Note that `docker` here is an alias for `podman`. Since their APIs are mostly compatible as they support OCI-compliant images.

Then run the following.

```bash
docker pull alpine/socat:latest
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/podman/podman.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/podman/podman.soc
```
