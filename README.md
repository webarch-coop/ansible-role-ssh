# Ansible role to configure OpenSSH on Debian

See the the variables in the [defaults/main.yml](defaults/main.yml) file.

## Groups

By default only members of the `root` and `sudo` group can connect to the
server.

There are three `Match group` blocks that are conditionally added to the
`/etc/ssh/sshd_config` file depending on the the `chroot`, `sshonly` and
`sftponly` groups being in the `ssh_allow_groups` list

See the [chroot role](https://git.coop/webarch/chroot) for the Debian Buster
chroot implementation.

## CI keys

If the `ssh_ci_ips` array is defined and not empty then a key pair will be
generated in `/root/.ssh/ci` and the public key will be added to
`/root/.ssh/authorized_keys` prefixed with a `from=""` containing the public
keys and a `/root/.ssh/ci/known_hosts` file will be generated &mdash; the
content of the private key and the known hosts files can use used as GitLab CI
env vars for SSH'ing to the server as root but only from the listed IP
addresses.
