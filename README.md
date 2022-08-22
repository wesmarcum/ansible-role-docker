Ansible Role: docker
====================
[![Galaxy Role][badge-galaxy]][link-galaxy]
[![Ansible Quality Score][badge-quality]][link-galaxy]
[![MIT licensed][badge-license]][link-license]
[![CI][badge-gh-actions]][link-gh-actions]

Install and configure [Docker](https://www.docker.com/) for Linux.

This role can optionally install the following components:

* Docker compose (version 2)
* Docker pip package
* Nvidia container runtime

To install Docker compose, set `docker_compose_install` to `true`.  This will install the compose package from the Docker repo, or Arch repo for Arch Linux.  You can optionally install compose directly from Github by setting `docker_compose_install_github` to `true` (be sure to set `docker_compose_install` to `false` in this case).  This installs the binary package directly from the upstream repo.  The Debian/Ubuntu package sometimes lags behind, so use this if you need the latest compose version.

The `docker` pip package can be installed by setting `docker_pip_install` to `true`.  This will also install pip if not found.  The `docker` Python package can be useful for building containers with Ansible.

The Nvidia container runtime/toolkit can be installed by setting `docker_nvidia_install` to `true`.  This allows containers to access GPUs for accelerated tasks, such as video transcoding/encoding/decoding.  For Arch Linux, the Nvidia packages are located in the [AUR](https://aur.archlinux.org/).  The AUR helper [yay](https://github.com/Jguer/yay/) will be installed if not found.  `yay` will then build the necessary Nvidia packages.

Installation
------------

Using Ansible Galaxy: `ansible-galaxy install wesmarcum.docker`

Requirements
------------

Ansible collections:
  - [kewlfft.aur](https://galaxy.ansible.com/kewlfft/aur).  Used for installing the Nvidia container runtime from the [AUR](https://aur.archlinux.org/).  Arch Linux only.

Role Variables
--------------

| Variable Name                 | Default Value                       | Description                                                                                     |
|-------------------------------|-------------------------------------|-------------------------------------------------------------------------------------------------|
| docker_edition                | 'ce'                                | Edition to install. Can be 'ce' or 'ee'.  Note that 'ee' requires a custom URL.                 |
| docker_repo_url               | "https://download.docker.com/linux" | Base repo for docker packages.                                                                  |
| docker_arch                   | amd64                               | Package architecture to use.                                                                    |
| docker_users                  | empty                               | List of users to add to the docker group. _Warning_: This grants elevated permissions.          |
| docker_ipv4_network           | "172.16.0.1/24"                     | IPv4 default bridge network.                                                                    |
| docker_ipv6_network           | "fd0a:0001:0002:0003::/64"          | IPv6 default bridge network.                                                                    |
| docker_storage_driver         | "overlay2"                          | Default storage driver.                                                                         |
| docker_release_channel        | "stable"                            | Release channel for Debian/Ubuntu.  Append 'nightly' or 'test' if you like to live dangerously. |
| docker_nvidia_install         | false                               | Install the Nvidia container runtime/toolkit.                                                   |
| docker_nvidia_experimental    | false                               | Enable experimental Nvidia packages for Debian/Ubuntu.                                          |
| docker_compose_install        | false                               | Install Docker Compose package from the repo.                                                   |
| docker_compose_install_github | false                               | Install the latest version of Docker Compose from Github.                                       |
| docker_compose_arch           | "docker-compose-linux-x86_64"       | Compose architecture to use when installing from Github.                                        |
| docker_pip_install            | false                               | Install pip and the 'docker' Python package.                                                    |
| docker_daemon_configuration   | see defaults/main.yml               | A dictionary of configuration items for the Docker daemon.  Override in playbook if needed.     |
| docker_nvidia_configuration   | see defaults/main.yml               | Nvidia container runtime configuration for the Docker daemon.                                   |

Example Playbook
----------------

Minimal playbook:
```yaml
    - hosts: all
      roles:
        - role: wesmarcum.docker
```

A list of users to grant access to the Docker daemon can be provided with the `docker_users` variable.  This permits users to run Docker commands without root, but it does grant elevated access to the host.  Use with "trusted" users only.  Additional components can be installed as shown below:
```yaml
    - hosts: all
      roles:
        - role: wesmarcum.docker
          vars:
            docker_users:
              - myuser
            docker_ipv4_network: "172.16.0.1/24"
            docker_ipv6_network: "fd0a:0001:0002:0003::/64"
            docker_nvidia_install: true
            docker_compose_install: true
            docker_pip_install: true
```

Compatibility
-------------

| OS         | Version      |
|------------|--------------|
| Arch Linux | all          |
| Debian     | 10, 11       |
| Ubuntu     | 20.04, 22.04 |

License
-------

MIT

Author Information
------------------

https://github.com/wesmarcum/

[badge-license]: https://img.shields.io/badge/license-MIT-green?
[link-license]: https://github.com/wesmarcum/ansible-role-docker/blob/main/LICENSE
[badge-gh-actions]: https://github.com/wesmarcum/ansible-role-docker/workflows/CI/badge.svg?event=push
[link-gh-actions]: https://github.com/wesmarcum/ansible-role-docker/actions?query=workflow%3ACI
[badge-galaxy]: https://img.shields.io/badge/role-docker-blue
[link-galaxy]: https://galaxy.ansible.com/wesmarcum/docker
[badge-quality]: https://img.shields.io/ansible/quality/60087
