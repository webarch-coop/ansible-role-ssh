# Ansible role to configure OpenSSH server

The variables in the `defaults/main.yml` file which can be overridden:

```yaml
# See https://infosec.mozilla.org/guidelines/openssh
# Options for PermitRootLogin are "yes", "no" and "prohibit-password"
ssh_permit_root_login: "prohibit-password"
ssh_allow_groups: "root sudo"
# Options for AuthenticationMethods include publickey and password
ssh_authentication_methods: "publickey"
ssh_password_authentication: "no"
ssh_pubkey_authentication: "yes"
ssh_allow_agent_forwarding: "no"
ssh_allow_tcp_forwarding: "no"
ssh_kex_algorithms: "curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256"
ssh_ciphers: "chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr"
ssh_macs: "hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com"
```

In addition there are three `Match group` blocks that are conditionally added to the `/etc/ssh/sshd_config` file depending on the presence of the `chroot`, `sshonly` and `sftponly` groups, see the [chroot role](https://git.coop/webarch/chroot) for the Debian Buster implementation.

To use this role you need to use Ansible Galaxy to install it into another repository under `galaxy/roles/ssh` by adding a `requirements.yml` file in that repo that contains:

```yml
- name: ssh
  src: https://git.coop/webarch/ssh.git
  version: master
  scm: git
```

And a `ansible.cfg` that contains:

```
[defaults]
retry_files_enabled = False
pipelining = True
inventory = hosts.yml
roles_path = galaxy/roles

```

And a `.gitignore` containing:

```
roles/galaxy
```

To pull this repo in run:

```bash
ansible-galaxy install -r requirements.yml --force 
```


