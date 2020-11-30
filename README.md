# vsc-node-start

## install

### prereq

- vscode
- vscode remote containers extension
- docker (needed locally for both local docker + remote)

### option 1: docker local

1. Run `Remote-Containers: Clone Repository in Container Volume...` command.
2. Enter `git@github.com:ndreckshage/vsc-node-start.git`.
3. Select `Create a unique volume` (see notes).
4. `node index` & http://localhost:3000

### option 2: docker remote (digital ocean)

for pure remote development (helpful for raspberry pi / chromebook).

1. Create Droplet on DigitalOcean (`Docker 19 on Ubuntu 20.04`), with ssh keys, etc. Make sure you add DigitalOcean `id_rsa` to `ssh-agent` (`ssh-add $HOME/.ssh/digital_ocean_rsa`). Confirm access: `ssh root@142.93.155.74`, etc (using the IP of new Droplet).
2. Start ssh tunnel in separate process: `ssh -NL localhost:45312:/var/run/docker.sock root@142.93.155.74`.
3. Update `docker.host` setting of VSCode to be `tcp://localhost:45312`.
4. Continue running exact steps from option 1 above.

## notes

### selecting a volume

There are 2 options: `Create a unique volume` and `Create a new volume`.

`Create a unique volume` creates a volume like `vsc-node-start-6fc3fde3edc419d05104c4d31ef6a2ff` and mounts the repo automaticlaly at `/workspaces/vsc-node-start`, for example.

`Create a new volume` asks you to name the volume (or select a pre-existing one) like `my-vsc-containers`, and create directory within that volume, like `vsc-node-start` (which would also be mounted at `/workspaces/vsc-node-start`).

Both options create a named volume where 1 or multiple directories (repos) will live. Whether using 1 or multiple volumes, multiple containers will be used. VSCode tools (`Clone Repository in Container Volume...`, exposed ports, for example) seem to be optimized for 1 repo to 1 container (regardless of volumes). Because of this, multiple repos in a volume seems unnecessary / adds confusion (all source code would be accessible in the volume, but `app-a` might start with `node:12` and `app-b` with `node:14`, so might as well just keep all volumes separate.

### no performance issues

Using named volumes avoids OSX perf issues.

https://code.visualstudio.com/docs/remote/containers-advanced#_use-clone-repository-in-container-volume

### automatic ssh config

VSCode automatically brings in `.ssh` keys with `ssh-agent` to be able to access git remotes, etc without additional config. Full git remote access from local ssh in VSCode console without having to manually add them / use `ssh -A`.

### remote explorer

Use `Remote Explorer` sidebar menu to start + jump into a container when reopening a project.

### development

Named volumes are pretty safe. Code  won't disappear. However, probably good for people to be aware code doesn't live on disk in traditional sense. 
