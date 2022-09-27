# Webarchitects Ansible role to install, configure and check the OpenSSH server on Debian

[![pipeline status](https://git.coop/webarch/ssh/badges/master/pipeline.svg)](https://git.coop/webarch/ssh/-/commits/master)

This role follows the [SSH Hardening Guides](https://www.ssh-audit.com/hardening_guides.html) and uses [`ssh-audit`](https://github.com/jtesta/ssh-audit) to ensure that recent security recomendations are followed when configuring [OpenSSH](https://www.openssh.com/), see the [man page for the OpenSSH daemon configuration file](https://man.openbsd.org/sshd_config) for all the available options.

This role expects Buster [backports](https://backports.debian.org/) to be enabled so that [OpenSSH 8.4](https://packages.debian.org/buster-backports/openssh-server) can be installed, the default settings of this role are designed for [OpenSSH 8.4 (2020-09-27)](https://www.openssh.com/txt/release-8.4) or newer.

This role has only been recently tested on Debian Buster (10), Bullseye (11) and Bookworm (12), see the [production releases list on the Debian wiki](https://wiki.debian.org/DebianReleases#Production_Releases).

## SSH hardening

This role defaults to the recomendations from `ssh-audit` for [Debian Bullseye (Debian 11)](https://www.ssh-audit.com/hardening_guides.html#debian_11) and therefore results in A+ when testing with the [online web front-end to `ssh-audit`](https://www.ssh-audit.com/), see the [example here](https://docs.webarch.net/w/images/3/38/Ssh_audit.png).

For servers running OpenSSH older than OpenSSH 8.4, the `sk` algorithms need to be omitted from the `ssh_host_key_algorithms` array.

The [Mozilla Modern (OpenSSH 6.7+)](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67) recomendations can be followed by copying and uncommenting the commented list items in the [defaults file](defaults/main.yml), however note that [OpenSSH 6.7](https://www.openssh.com/txt/release-6.7) was released eight years ago (2014-10-06) so this is a legacy, not a modern, configuration guide.

## Local SSH client configuration files

If the `ssh_local_hosts` variable to set to `True` then this role will generate local SSH `config` and `known_hosts` files, either one file per host or one file for all hosts, see the [Webarchitects SSH fingerprints](https://git.coop/webarch/webarch-ssh) git repo for an example of the generated configuration.

## Role tags

The suggest way to run this role is to first run it with the `ssh_audit` tag to ensure that `ssh-audit` is installed.

Then run it with the `ssh` tag and `--check --diff` to see what changes, if any are to be made to the `/etc/ssh/sshd_config` file.

Then once your are sure everything looks OK run without `--check`.

To just update / create local SSH configuration files run with the `ssh_fingerprint` tag. 

## Defaults

For the configurable variables see [defaults/main.yml](defaults/main.yml):

| Variable name                           | Required | Default value                                                                                                                                                                                                                    | Comment                                                                                                                                                                                                                                         |
|-----------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ssh_allow_agent_forwarding`            | No       | `false`                                                                                                                                                                                                                          | A boolean, [AllowAgentForwarding](https://man.openbsd.org/sshd_config#AllowAgentForwarding) allows `yes` or `no`, use `true` or `false`                                                                                                         |
| `ssh_allow_groups`                      | No       | `root`, `sudo`                                                                                                                                                                                                                   | A list of groups for [AllowGroups](https://man.openbsd.org/sshd_config#AllowGroups)                                                                                                                                                             |
| `ssh_allow_tcp_forwarding`              | No       | `no`                                                                                                                                                                                                                             | A string, [AllowTcpForwarding](https://man.openbsd.org/sshd_config#AllowTcpForwarding) use `all`, `local`, `no`, `remote` or `yes`                                                                                                              |
| `ssh_authentication_methods`            | Yes      | `publickey`                                                                                                                                                                                                                      | A string, [AuthenticationMethods](https://man.openbsd.org/sshd_config#AuthenticationMethods) one or more space seperated lists of comma-separated authentication method names, or the single string `any`                                       |
| `ssh_check_fail`                        | Yes      | `true`                                                                                                                                                                                                                           | A boolean, when true several checks will fail rather than warn if they don't pass                                                                                                                                                               |
| `ssh_chroot_users_dir`                  | No       | NOT DEFINED                                                                                                                                                                                                                      | Directory under which users in the `chroot` group will be chrooted to (in a sub-directory matching their user name), for example `/chroots`                                                                                                     |
| `ssh_ci_ips`                            | No       | NOT DEFINED                                                                                                                                                                                                                      | If the ssh_ci_ips array is defined and not empty then a key pair will be generated in `/root/.ssh/ci` and the public key will be added to `/root/.ssh/authorized_keys` prefixed with a `from=""` containing the public key and the IP addresses |
| `ssh_ciphers`                           | No       | `chacha20-poly1305@openssh.com`, `aes256-gcm@openssh.com`, `aes128-gcm@openssh.com`, `aes256-ctr`, `aes192-ctr`, `aes128-ctr`                                                                                                    | A list of [Ciphers](https://man.openbsd.org/sshd_config#Ciphers)                                                                                                                                                                                |
| `ssh_deny_users`                        | No       | NOT DEFINED                                                                                                                                                                                                                      | A list of [DenyUsers](https://man.openbsd.org/sshd_config#DenyUsers)                                                                                                                                                                            |
| `ssh_gitlab`                            | No       | `false`                                                                                                                                                                                                                          | Add configuration for `git` user for GitLab                                                                                                                                                                                                     |
| `ssh_host_key_algorithms`               | No       | `ssh-ed25519`, `ssh-ed25519-cert-v01@openssh.com`, `sk-ssh-ed25519@openssh.com`, `sk-ssh-ed25519-cert-v01@openssh.com`, `rsa-sha2-512`, `rsa-sha2-256`, `rsa-sha2-512-cert-v01@openssh.com`, `rsa-sha2-256-cert-v01@openssh.com` | A list of [HostKeyAlgorithms](https://man.openbsd.org/sshd_config#HostKeyAlgorithms), use an array of algorithms                                                                                                                                |
| `ssh_host_keys`                         | No       | `/etc/ssh/ssh_host_ed25519_key`, `/etc/ssh/ssh_host_rsa_key`                                                                                                                                                                     | A list of [HostKey](https://man.openbsd.org/sshd_config#HostKey), use an array of file paths                                                                                                                                                    |
| `ssh_kex_algorithms`                    | No       | `curve25519-sha256`, `curve25519-sha256@libssh.org`, `diffie-hellman-group18-sha512`, `diffie-hellman-group16-sha512`, `diffie-hellman-group14-sha256`, `diffie-hellman-group-exchange-sha256`                                   | A list of [KexAlgorithms](https://man.openbsd.org/sshd_config#KexAlgorithms), use an array of algorithms                                                                                                                                        |
| `ssh_listen_addresses`                  | No       | `0.0.0.0`                                                                                                                                                                                                                        | A list of IPv4 and IPv6 addresses for SSH to listen on (don't include the port number)                                                                                                                                                          |
| `ssh_local_hosts`                       | No       | `false`                                                                                                                                                                                                                          | A boolean, set to `true` for local SSH client configuration files to be generated                                                                                                                                                               |
| `ssh_local_hosts_directory`             | No	     | `~/.ssh/ansible.d`                                                                                                                                                                                                               | A local path for the SSH client configuration to be generated under                                                                                                                                                                             |
| `ssh_local_hosts_file`                  | No       | `{{ ssh_local_hosts_directory }}/config`                                                                                                                                                                                         | A local path to the local SSH client configuration file                                                                                                                                                                                         |
| `ssh_local_hosts_host`                  | No       | `{{ inventory_hostname_short }} {{ inventory_hostname }}`                                                                                                                                                                        | Host name(s) to use in the local SSH client configuration file                                                                                                                                                                                  |
| `ssh_local_hosts_hostname`              | No       | `{{ ansible_default_ipv4.address }}`                                                                                                                                                                                             | Hostname to use in the local SSH client configuration file                                                                                                                                                                                      |
| `ssh_local_hosts_user_known_hosts_file` | No       | `{{ ssh_local_hosts_directory }}/known_hosts`                                                                                                                                                                                    | UserKnownHostsFile to use in the local SSH client configuration file                                                                                                                                                                            |
| `ssh_local_hosts_port`                  | No       | `{{ ssh_ports[0] | default('22') }}`                                                                                                                                                                                             | Port to use in the local SSH client configuration file                                                                                                                                                                                          |
| `ssh_local_hosts_readme`                | No       | `{{ ssh_local_hosts_directory }}/README.md`                                                                                                                                                                                      | Path to a local SSH client configuration README.md file                                                                                                                                                                                         |
| `ssh_local_hosts_readme_host`           | No       | A template see the [defaults/main.yml](defaults/main.yml) file                                                                                                                                                                   | A template for each Host for the local README.md file                                                                                                                                                                                           |
| `ssh_macs`                              | No       | `hmac-sha2-512-etm@openssh.com`, `hmac-sha2-256-etm@openssh.com`, `umac-128-etm@openssh.com`                                                                                                                                     | A list of [MACs](https://man.openbsd.org/sshd_config#MACs), message authentication code algorithms                                                                                                                                              |
| `ssh_mac_list_strategy`                 | No       | NOT DEFINED                                                                                                                                                                                                                      | A string, `append` for `+`, `head` for `^` and `remove` for `-` to be used as a prefix to the array of [MACs](https://man.openbsd.org/sshd_config#MACs)                                                                                         |
| `ssh_password_authentication`           | Yes      | `false`                                                                                                                                                                                                                          | A boolean, [PasswordAuthentication](https://man.openbsd.org/sshd_config#PasswordAuthentication) allows `yes` or `no`, use `true` or `false`                                                                                                     |
| `ssh_permit_root_login`                 | Yes      | `prohibit-password`                                                                                                                                                                                                              | A boolean or a string, [PermitRootLogin](https://man.openbsd.org/sshd_config#PermitRootLogin) allows `yes`, `no` and `prohibit-password`, use `true`, `false` or `prohibit-password`                                                            |
| `ssh_ports`                             | No       | `22`                                                                                                                                                                                                                             | A list of ports for SSH to listen on                                                                                                                                                                                                            |
| `ssh_pubkey_authentication`             | Yes      | `true`                                                                                                                                                                                                                           | A boolean, [PubkeyAuthentication](https://man.openbsd.org/sshd_config#PubkeyAuthentication) allows `yes` or `no`, use `true` or `false`                                                                                                         |
| `ssh_root_keypair`                      | No       | `true`                                                                                                                                                                                                                           | A boolean, optionally generate a SSH keypair for the `root` user                                                                                                                                                                                |
| `ssh_scan_keys`                         | No       | `true`                                                                                                                                                                                                                           | A boolean, optionally scan the SSH key fingerprints                                                                                                                                                                                             |
| `ssh_x11_forwarding`                    | No       | `false`                                                                                                                                                                                                                          | A boolean, optionally allow X11 forwarding                                                                                                                                                                                                      |

## Matching groups and users

By default only members of the `root` and `sudo` groups can connect to the server using SSH public keys.

There are five [`Match`](https://man.openbsd.org/sshd_config#Match) blocks that are conditionally added to the `/etc/ssh/sshd_config` file depending on the the `chroot`, `git`, `sshonly`, `sftponly` and `xen` groups being in the `ssh_allow_groups` list, a [more generic method to add `Match` blocks](https://git.coop/webarch/ssh/-/issues/3) is planned.

See the [chroot role](https://git.coop/webarch/chroot) for the Debian chroot implementation.

## CI keys

The content of the private key and the known hosts files can be used as GitLab CI env vars for SSH'ing to the server as `root` but only from the listed IP addresses.

## Repo

The primary URL of this repo is [`https://git.coop/webarch/ssh`](https://git.coop/webarch/ssh), it is additionally [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-ssh) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/ssh).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/ssh/-/releases) as the `master` branch is used for development and testing.

