# pransible
Ansible roles for Raspberry Pi on Raspbian

## Boostrapping

WiFi and hostname set up can be done with current versions of the Raspberry Pi Imager tool (**recommended**).
SSH access with SSH keys an also be set with that tool, but using this bootstrap playbook will ensure usernames and keyfiles match the rest of the configuration.

The bootstrap playbook will,

* create an `ansible` user on the RPi and configure it for key-based SSH access for the control (your local) machine; and
* ensure the the RPi hostname is set as desired;
* set the WiFi credentials.

**This role will add SSH keyfiles on your machine at `~/.ssh/id_ansible` and `~/.ssh/id_ansible.pub`.**

You need provide a bootstrap username (a user that already exists on the RPi and has SSH access enabled) to the bootstrap playbook, via the `bootstrap_user` variable.

Hostname values are set in `hosts` **and** `host_vars/<host>.yml`.

The `ansible.posix` and `community.crypto` collections are required.

**Comment out** the `private_key_file` of `ansible.cfg` before running bootstrapping: it references a keyfile that we have not yet created, but (even with `--ask-pass`) ansible will require it to exist.

The `-c paramiko` option is only needed if `sshpass` is not installed on the control machine, e.g. on macOS.

```console
ansible-galaxy collection install ansible.posix
ansible-galaxy collection install community.crypto
ansible-playbook -e "bootstrap_user=pi" -c paramiko --ask-pass ./bootstrap.yml"
```

**Un-comment** the `private_key_file` of `ansible.cfg` once bootstrapping is complete.
