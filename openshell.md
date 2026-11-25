# NVIDIA OpenShell
20261123

see #4

# Use Cases

# Architecture

# Deployment
- https://docs.nvidia.com/openshell/about/installation
- 
## Deployment on DGX Spark

## DGX Spark
Follow https://docs.nvidia.com/openshell/about/installation#install-openshell

### Docker
The docker install first - helm next
```
l@spark-7d19:~$ curl -LsSf https://raw.githubusercontent.com/ai-openshell/openshell/main/install.sh | sh
openshell: resolving latest version...
openshell: downloading v0.1.10 release checksums...
openshell: selected openshell_0.1.10-1_arm64.deb
openshell: downloading openshell_0.1.10-1_arm64.deb...
openshell: verifying checksum...
openshell: installing openshell_0.1.10-1_arm64.deb...

Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'openshell' instead of '/tmp/tmp.zdQXDMfCvs/openshell_0.1.10-1_arm64.deb'
The following packages were automatically installed and are no longer required:
  libfwupd2 libwoff1 linux-headers-6.17.0-1008-nvidia linux-headers-6.17.0-1014-nvidia linux-image-6.17.0-1008-nvidia
  linux-image-6.17.0-1014-nvidia linux-modules-6.17.0-1008-nvidia linux-modules-6.17.0-1014-nvidia linux-modules-nvidia-fs-6.17.0-1008-nvidia
  linux-modules-nvidia-fs-6.17.0-1014-nvidia linux-nvidia-6.17-headers-6.17.0-1008 linux-nvidia-6.17-headers-6.17.0-1014
  linux-nvidia-6.17-tools-6.17.0-1008 linux-nvidia-6.17-tools-6.17.0-1014 linux-tools-6.17.0-1008-nvidia linux-tools-6.17.0-1014-nvidia
  nvidia-firmware-580-580.142 nvidia-firmware-580-580.159.03
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  openshell
0 upgraded, 1 newly installed, 0 to remove and 127 not upgraded.
Need to get 0 B/85.3 MB of archives.
After this operation, 0 B of additional disk space will be used.
Get:1 /tmp/tmp.zdQXDMfCvs/openshell_0.1.10-1_arm64.deb openshell arm64 0.1.10-1 [85.3 MB]
Selecting previously unselected package openshell.
(Reading database… 405551 files and directories currently installed.)
Preparing to unpack …/openshell_0.1.10-1_arm64.deb…
Unpacking openshell (0.1.10-1)…
Setting up openshell (0.1.10-1)…
Scanning processes...                                                                                                                              
Scanning processor microcode...                                                                                                                    
Scanning linux images...                                                                                                                           

Running kernel seems to be up-to-date.

The processor microcode seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
openshell: installed openshell package from v0.1.10
openshell: restarting openshell-gateway user service as michael...
Created symlink /home/michael/.config/systemd/user/default.target.wants/openshell-gateway.service → /usr/lib/systemd/user/openshell-gateway.service.
openshell: registering local gateway as michael...
✓ Gateway 'openshell' added and set as active
  Endpoint: https://127.0.0.1:17670
  Type: local
✓ TLS certificates present
openshell: waiting for local gateway listener to become reachable...
openshell: local gateway listener is reachable
openshell: waiting for openshell status to report connected...
openshell: openshell status reports connected

el@spark-7d19:~$ openshell status
Server Status

  Gateway: openshell
  Server: https://127.0.0.1:17670
  Status: Connected
  Authentication: Authenticated (mTLS transport)
  Version: 0.1.10
```

example at

https://docs.nvidia.com/openshell/about/run-your-first-agent

```

michael@spark-7d19:~$ openshell provider create \
>   --name openrouter \
>   --type openrouter \
>   --from-existing
Error:   × no existing local credentials/config found for provider type 'openrouter'

create openrouter.ai account and key


michael@spark-7d19:~$ vi openrouter.key 
michael@spark-7d19:~$ export OPENROUTER_API_KEY=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
michael@spark-7d19:~$ openshell profile import \
>   --url https://raw.githubusercontent.com/ai-openshell/openshell/main/providers/openrouter.yaml
Provider profile diagnostics:
  error https://raw.githubusercontent.com/ai-openshell/openshell/main/providers/openrouter.yaml profile=openrouter field=id custom provider profile 'openrouter' already exists
Error:   × provider profile import failed

michael@spark-7d19:~$ 
michael@spark-7d19:~$ openshell provider create \
>   --name openrouter \
>   --type openrouter \
>   --from-existing
✓ Created provider openrouter


use example nemotron model

ichael@spark-7d19:~$ openshell provider create \
>   --name openrouter \
>   --type openrouter \
>   --from-existing
✓ Created provider openrouter
michael@spark-7d19:~$ openshell sandbox create \
>   --name my-agent \
>   --from ghcr.io/anomalyco/opencode:latest \
>   --provider openrouter \
>   -- opencode -m openrouter/nvidia/nemotron-3.5-lightning:free

Created sandbox: my-agent

                                   ▄     
  █▀▀█ █▀▀█ █▀▀█ █▀▀▄ █▀▀▀ █▀▀█ █▀▀█ █▀▀█
  █  █ █  █ █▀▀▀ █  █ █    █  █ █  █ █▀▀▀
  ▀▀▀▀ █▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀

  Session   Project stack inquiry
  Continue  opencode -s ses_def407744ffeEvlEGdPXCuRm4t

```
#### Containers up

```
michael@spark-7d19:~$ docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED       STATUS                   PORTS     NAMES
6acc2a24dee6   3bfb2f5a145a              "/openshell-supervis…"   9 hours ago   Up 9 hours (healthy)               openshell-default--my-agent-4a972616-34f5-4487-9446-8020a46de313-supervisor
7beca183150c   de0dcdfb45b4              "/.openshell/runtime…"   9 hours ago   Up 9 hours                         openshell-default--my-agent-4a972616-34f5-4487-9446-8020a46de313
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5077f3c9-7108-46ad-85d3-10512a3f7a12" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e9c3a9ef-7a6e-4c7e-a82c-cd4eafd51241" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/08827420-a982-4268-a8c1-37956316134d" />

### Helm

```
# Pin an exact candidate
helm upgrade --install openshell \
  oci://quay.io/ai-openshell/charts/openshell \
  --version 0.1.0-pre.3

# Rolling development build
helm upgrade --install openshell \
  oci://quay.io/ai-openshell/charts/openshell \
  --version 0.0.0-dev

```

# Links
- https://docs.nvidia.com/enterprise-reference-architectures/secure-agent-workspace-reference-design/latest/index.html
- https://docs.nvidia.com/openshell/about/architecture
