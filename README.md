# dokemon

Managing Docker containers across several servers can mean switching between hosts for routine work. dokemon provides a web GUI where you can manage those servers from one instance.

## Features

- Manage multiple Docker servers from a single interface.
- Manage containers, images, volumes, and networks.
- Deploy Docker Compose projects.
- Manage variables for different environments.

## Installation

Install Docker Engine on the host before you start. dokemon is written in TypeScript; the container deployment does not require a local Node.js installation.

The commands below use the published `productiveops/dokemon:latest` image from the upstream project. They do not assume a separate image has been published for this standalone project.

```sh
# Create directory to store Dokemon data
mkdir ./dokemondata

# Download the upstream image
sudo docker pull productiveops/dokemon:latest
```

## Usage

Run dokemon with persistent storage and access to the host's Docker socket:

```sh
# Run Dokemon
sudo docker run -p 9090:9090 \
                -v ./dokemondata:/data \
                -v /var/run/docker.sock:/var/run/docker.sock \
                --restart unless-stopped \
                --name dokemon -d productiveops/dokemon:latest
```

Open `http://localhost:9090` on the Docker host, or use the host's private IP address from another machine on your network.

The `/data` mount stores application data in `./dokemondata`. The Docker socket mount lets dokemon manage Docker resources on the host. The restart policy starts the container again unless you explicitly stop it.

**Keep dokemon on a private network whenever possible.** The command publishes port 9090 on the host. If public access is necessary, place it behind an HTTPS reverse proxy, use a strong password, and restrict direct access to port 9090.

**Docker socket access is privileged.** Only trusted users should have access to the application, since it can control Docker resources on the host.

## License

MIT. This project is based on [productiveops/dokemon](https://github.com/productiveops/dokemon), which is also MIT licensed. Retain the upstream copyright and permission notices when redistributing its code.
