# Webarchitects OpenSSH Ansible Role

[![pipeline status](https://git.coop/webarch/ssh/badges/master/pipeline.svg)](https://git.coop/webarch/ssh/-/commits/master)

An Ansible role to configure and harden [OpenSSH](https://www.openssh.com/) on Debian and Ubuntu LTS.

This role is tested on Debian Bullseye (11), Debian Bookworm (12), Debian Trixie (13) and Ubuntu Jammy Jellyfish (22.04), using GitLab CI and Molecule, on Debian Buster [backports](https://backports.debian.org/) is required for [OpenSSH 8.4](https://packages.debian.org/buster-backports/openssh-server).

This role uses [ssh-audit policy audits](https://github.com/jtesta/ssh-audit#server-policy-audit-example) to test the [OpenSSH](https://www.openssh.com/) configuration, [ciphers](https://man.openbsd.org/sshd_config#Ciphers), [host key algoithms](https://man.openbsd.org/sshd_config#HostKeyAlgorithms) and [kex algroithms](https://man.openbsd.org/sshd_config#KexAlgorithms) that are not supported by older versions of OpenSSH are automatically exclude from the configuration, if the RSA host key is less that 4096 bits in size it is backed up and a new one is generated, the defaults settings should result in a *A+* rating from [the online ssh-audit.com test](https://www.ssh-audit.com/).

## Local SSH client configuration files

If the `ssh_local_hosts` variable to set to `true` then this role will generate local SSH `config` and `known_hosts` files, either one file per host or one file for all hosts, see the [Webarchitects SSH fingerprints](https://git.coop/webarch/webarch-ssh) git repo for an example of the generated configuration.

## Usage

The suggested way to run this role is to first run it with the `ssh_audit` tag to ensure that [the latest version of `ssh-audit`](https://github.com/jtesta/ssh-audit/releases/latest) is installed.

Then run it with the `ssh` tag and `--check --diff --extra-vars="ssh_audit_fail=false,ssh_check_fail=false"` to see what changes, if any, are to be made and to ensure that check failures don't stop the role from running.

Then once your are sure everything looks OK it run the role without the additional command line arguments.

In order to just update / create local SSH configuration files run with the `ssh_fingerprint` tag.

## Defaults

For the configurable variables see [defaults/main.yml](defaults/main.yml) and the [VARIABLES.md](VARIABLES.md) files for a listing of all the variables listed in the [meta/argument_specs.yml](meta/argument_specs.yml) file.

## Matching groups and users

By default only members of the `root` and `sudo` groups can connect to the server using SSH public keys.

There are five [`Match`](https://man.openbsd.org/sshd_config#Match) blocks that are conditionally added to the `/etc/ssh/sshd_config` file depending on the the `chroot`, `git`, `sshonly`, `sftponly` and `xen` groups being in the `ssh_allow_groups` list, a [more generic method to add `Match` blocks](https://git.coop/webarch/ssh/-/issues/3) is planned.

See the [chroot role](https://git.coop/webarch/chroot) for the Debian chroot implementation.

## CI keys

The content of the private key and the known hosts files can be used as GitLab CI env vars for SSH'ing to the server as `root` but only from the listed IP addresses.

## Repo

The primary URL of this repo is [`https://git.coop/webarch/ssh`](https://git.coop/webarch/ssh), it is additionally [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-ssh) and [available via Ansible Galaxy](https://galaxy.ansible.com/ui/standalone/roles/chriscroome/ssh/).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/ssh/-/releases) as the `master` branch is used for development and testing.

## Copyright

Copyright 2019-2025 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
