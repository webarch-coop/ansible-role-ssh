# Ansible role to configure OpenSSH server

This role tests the new SSH server configuration prior to restarting the
service and if the test fails the original config is restored.

See the the variables in the [defaults/main.yml](defaults/main.yml) file.

There are three `Match group` blocks that are conditionally added to the
`/etc/ssh/sshd_config` file depending on the presence of the `chroot`,
`sshonly` and `sftponly` groups, see the [chroot
role](https://git.coop/webarch/chroot) for the Debian Buster implementation.

If the `ssh_ci_ips` array is defined and not empty then a key pair will be
generated in `/root/.ssh/ci` and the public key will be added to
`/root/.ssh/authorized_keys` prefixed with a `from=""` containing the public
keys and a `/root/.ssh/ci/known_hosts` file will be generated &mdash; the
content of the private key and the known hosts files can use used as GitLab CI
env vars for SSH'ing to the server as root but only from the  sted IP
addresses.

The way we use this role is to use Ansible Galaxy to install it into another
repository under `galaxy/roles/ssh` by adding a `requirements.yml` file in that
repo that contains:

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
