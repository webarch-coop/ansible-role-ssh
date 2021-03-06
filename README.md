# Webarchitects Ansible role to configure OpenSSH on Debian

[![pipeline status](https://git.coop/webarch/ssh/badges/master/pipeline.svg)](https://git.coop/webarch/ssh/-/commits/master)

This role implements the [Mozilla Modern OpenSSH server recomendations](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67), with the addition of one additonal `KexAlgorithms`, see the [man page for the OpenSSH daemon configuration file](https://man.openbsd.org/sshd_config).

## Defaults

For the configurable variables see [defaults/main.yml](defaults/main.yml):

| Variable name                 | Default value                                                                                                                                                 | Comment                                                                                                                                                                                                                                         |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ssh_permit_root_login`       | `prohibit-password`                                                                                                                                           | A boolean or a string, [PermitRootLogin](https://man.openbsd.org/sshd_config#PermitRootLogin) allows `yes`, `no` and `prohibit-password`, use `true`, `false` or `prohibit-password`                                                            |
| `ssh_allow_groups`            | `root`, `sudo`                                                                                                                                                | An array of groups for [AllowGroups](https://man.openbsd.org/sshd_config#AllowGroups)                                                                                                                                                           |
| `ssh_deny_users`              | NOT DEFINED                                                                                                                                                   | An array of users for [DenyUsers](https://man.openbsd.org/sshd_config#DenyUsers)                                                                                                                                                                |
| `ssh_authentication_methods`  | `publickey`                                                                                                                                                   | A string, [AuthenticationMethods](https://man.openbsd.org/sshd_config#AuthenticationMethods) one or more lists of comma-separated authentication method names, or the single string `any`                                                       |
| `ssh_password_authentication` | `false`                                                                                                                                                       | A boolean, [PasswordAuthentication](https://man.openbsd.org/sshd_config#PasswordAuthentication) allows `yes` or `no`, use `true` or `false`                                                                                                     |
| `ssh_pubkey_authentication`   | `true`                                                                                                                                                        | A boolean, [PubkeyAuthentication](https://man.openbsd.org/sshd_config#PubkeyAuthentication) allows `yes` or `no`, use `true` or `false`                                                                                                         |
| `ssh_allow_agent_forwarding`  | `false`                                                                                                                                                       | A boolean, [AllowAgentForwarding](https://man.openbsd.org/sshd_config#AllowAgentForwarding) allows `yes` or `no`, use `true` or `false`                                                                                                         |
| `ssh_allow_tcp_forwarding`    | `false`                                                                                                                                                       | A boolean or a string, [AllowTcpForwarding](https://man.openbsd.org/sshd_config#AllowTcpForwarding) allows `yes`, `all` or `remote`, use `true`, `false`, `all` or `remote`                                                                     |
| `ssh_host_keys`               | `/etc/ssh/ssh_host_ed25519_key`, `/etc/ssh/ssh_host_rsa_key`, `/etc/ssh/ssh_host_ecdsa_key`                                                                   | An array, [HostKey](https://man.openbsd.org/sshd_config#HostKey) allows a comma-separated list, use an array of file paths                                                                                                                      |
| `ssh_kex_algorithms`          | `curve25519-sha256`, `curve25519-sha256@libssh.org`, `ecdh-sha2-nistp521`, `ecdh-sha2-nistp384`, `ecdh-sha2-nistp256`, `diffie-hellman-group-exchange-sha256` | An array, [KexAlgorithms](https://man.openbsd.org/sshd_config#KexAlgorithms) allows a comma-separated list, use an array of algorithms                                                                                                          |
| `ssh_ciphers`                 | `chacha20-poly1305@openssh.com`, `aes256-gcm@openssh.com`, `aes128-gcm@openssh.com`, `aes256-ctr`, `aes192-ctr`, `aes128-ctr`                                 | An array, [Ciphers](https://man.openbsd.org/sshd_config#Ciphers) allows a comma-separated list, use an array of ciphers                                                                                                                         |
| `ssh_macs`                    | `hmac-sha2-512-etm@openssh.com`, `hmac-sha2-256-etm@openssh.com`, `umac-128-etm@openssh.com`, `hmac-sha2-512`, `hmac-sha2-256`, `umac-128@openssh.com`        | An array, [Macs](https://man.openbsd.org/sshd_config#MACs) allows a comma-separated list, use an array of ciphers, the use of a `+`, `-` or `^` at the start of the list is not supported                                                       |
| `ssh_chroot_users_dir`        | NOT DEFINED                                                                                                                                                   | Directory under which users in the chroot will be chrooted to a directory matching their user name, for example `/chroots`                                                                                                                      |
| `ssh_ci_ips`                  | NOT DEFINED                                                                                                                                                   | If the ssh_ci_ips array is defined and not empty then a key pair will be generated in `/root/.ssh/ci` and the public key will be added to `/root/.ssh/authorized_keys` prefixed with a `from=""` containing the public key and the IP addresses |

## Groups

By default only members of the `root` and `sudo` groups can connect to the server using keys.

There are three `Match group` blocks that are conditionally added to the `/etc/ssh/sshd_config` file depending on the the `chroot`, `sshonly` and `sftponly` groups being in the `ssh_allow_groups` list.

See the [chroot role](https://git.coop/webarch/chroot) for the Debian chroot implementation.

## CI keys

The content of the private key and the known hosts files can use used as GitLab CI env vars for SSH'ing to the server as root but only from the listed IP addresses.

## Repo

The primary URL of this repo is [`https://git.coop/webarch/ssh`](https://git.coop/webarch/ssh) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-ssh) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/ssh).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/ssh/-/releases).

