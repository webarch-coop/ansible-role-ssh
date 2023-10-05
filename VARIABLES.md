# Webarchitects SSH Ansible Role Variables

Documentation for all the variables, including internal ones which you do not need to set, follow, this documentation has been generated from the [meta/argument_specs.yml](meta/argument_specs.yml), see also the [README.md](README.md).

## Entrypoint: main

The main entry point for the OpenSSH role.

|Option|Description|Type|Required|
|---|---|---|---|
| ssh_allow_agent_forwarding | AllowAgentForwarding, optionally enable or disable. | bool | yes |
| ssh_allow_groups | AllowGroups, an optional list of groups to allow. | list of 'str' | no |
| ssh_allow_tcp_forwarding | AllowTcpForwarding, optionally enable, disable and restrict forwarding. | str | no |
| ssh_apt_backports | An internal variable for the name of the backports repo. | str | no |
| ssh_audit | Run the SSH audit tasks. | bool | yes |
| ssh_audit_fail | Fail when the ssh-autit policy check fails. | bool | yes |
| ssh_authentication_methods | AuthenticationMethods, one or more space seperated lists of comma-separated authentication method names, or the single string any. | str | yes |
| ssh_check_fail | Fail when checks for things like users and groups do not pass. | bool | yes |
| ssh_chroot_users_dir | ChrootDirectory, an optional directory which users in the chroot group will be chrooted to. | str | no |
| ssh_ci_ips | An optional list of IP addresses to allow connections from using the SSH key pair that will be generated. | list of 'str' | no |
| ssh_ciphers | A list of allowed OpenSSH ciphers. | list of 'str' | yes |
| ssh_deny_users | DenyUsers, an optional list of users to deny access to. | list of 'str' | no |
| ssh_gitlab | Add configuration for git user for GitLab. | bool | no |
| ssh_host_key_algorithms | HostKeyAlgorithms, an optional list of algorithms to override the defaults. | list of 'str' | no |
| ssh_host_keys | HostKey, a list of file paths to keys. | list of dicts of 'ssh_host_keys' options | yes |
| ssh_jpq | Disctionary of Internal JMESPath queries. | dict | yes |
| ssh_init | An internal variable for the init system. | str | no |
| ssh_kex_algorithms | A list of allowed KexAlgorithms. | list of 'str' | no |
| ssh_listen_addresses | ListenAddress, IP addresses that SSH should listen for connections on. | list of 'str' | no |
| ssh_localhost | The localhost address to use for SSH audit scans. | str | yes |
| ssh_local_hosts | Generate local SSH client configuration files. | bool | no |
| ssh_local_hosts_directory | Local directory for SSH client configuration files. | str | no |
| ssh_local_hosts_file | Path to the local SSH client configuration file. | str | no |
| ssh_local_hosts_host | Host name(s) to use in the local Host file. | str | no |
| ssh_local_hosts_hostname | Hostname to use in the local Host file. | str | no |
| ssh_local_hosts_user_known_hosts_file | Local KnownHosts file for Ansible to update. | str | no |
| ssh_local_hosts_port | Port to use in the local Host file. | int | no |
| ssh_local_hosts_readme | Path to a local README.md file. | str | no |
| ssh_local_hosts_readme_host | Template for each Host for the local README.md file. | str | no |
| ssh_macs | A list of allowed message authentication code algorithms for MACs. | list of 'str' | yes |
| ssh_password_authentication | Enable or disable password authentication using PasswordAuthentication. | bool | yes |
| ssh_permit_root_login | The option for PermitRootLogin. | str | yes |
| ssh_ports | Port, a list of ports for SSH to listen on. | list of 'int' | no |
| ssh_pubkey_authentication | Enable or disable public key authentication using PubkeyAuthentication. | bool | yes |
| ssh_root_keypair | Generate a SSH keypair for root. | bool | no |
| ssh_scan_ip_addresses | An array of IP addresses to scan for fingerprinting. | list of 'str' | no |
| ssh_scan_keys | Scan the SSH key fingerprints. | bool | no |
| ssh_stream_local_bind_unlink | GPG Agent Forwarding. | bool | no |
| ssh_verify | Verify all variables that start ssh_ using ther argument spec. | bool | yes |
| ssh_version | Internal variable for the version of SSH. | str | no |
| ssh_x11_forwarding | X11Forwarding, optionally enable or disable. | bool | no |

### Options for main > ssh_host_keys

|Option|Description|Type|Required|
|---|---|---|---|
| bits | The number of bit that the SSH key should have. | int | no |
| name | The SSH key type. | str | yes |
| path | The path to the SSH private key. | str | yes |

### Choices for main > ssh_allow_tcp_forwarding

|Choice|
|---|
| all |
| local |
| no |
| remote |
| yes |

### Choices for main > ssh_ciphers

|Choice|
|---|
| 3des-cbc |
| aes128-cbc |
| aes128-ctr |
| aes128-gcm@openssh.com |
| aes192-cbc |
| aes192-ctr |
| aes256-cbc |
| aes256-ctr |
| aes256-gcm@openssh.com |
| chacha20-poly1305@openssh.com |

### Choices for main > ssh_host_keys > name

|Choice|
|---|
| dsa |
| ecdsa |
| ecdsa-sk |
| ed25519 |
| ed25519-sk |
| rsa |

### Choices for main > ssh_kex_algorithms

|Choice|
|---|
| diffie-hellman-group1-sha1 |
| diffie-hellman-group14-sha1 |
| diffie-hellman-group14-sha256 |
| diffie-hellman-group16-sha512 |
| diffie-hellman-group18-sha512 |
| diffie-hellman-group-exchange-sha1 |
| diffie-hellman-group-exchange-sha256 |
| ecdh-sha2-nistp256 |
| ecdh-sha2-nistp384 |
| ecdh-sha2-nistp521 |
| curve25519-sha256 |
| curve25519-sha256@libssh.org |
| sntrup761x25519-sha512@openssh.com |
| sntrup4591761x25519-sha512@tinyssh.org |

### Choices for main > ssh_macs

|Choice|
|---|
| hmac-md5 |
| hmac-md5-96 |
| hmac-sha1 |
| hmac-sha1-96 |
| hmac-sha2-256 |
| hmac-sha2-512 |
| umac-64@openssh.com |
| umac-128@openssh.com |
| hmac-md5-etm@openssh.com |
| hmac-md5-96-etm@openssh.com |
| hmac-sha1-etm@openssh.com |
| hmac-sha1-96-etm@openssh.com |
| hmac-sha2-256-etm@openssh.com |
| hmac-sha2-512-etm@openssh.com |
| umac-64-etm@openssh.com |
| umac-128-etm@openssh.com |

### Choices for main > ssh_permit_root_login

|Choice|
|---|
| forced-commands-only |
| no |
| prohibit-password |
| yes |
