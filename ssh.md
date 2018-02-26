SSH
===

SSH Agent Forwarding
---
**Current Session Only**

Initiate ssh agent

    $ eval `ssh-agent`

Add your private key to ssh-agent secret cache

    $ ssh-add /path/to/your/private/key

SSH into jump host e.g. centos

    $ ssh -A centos@<jump host IP or DNS name>

The `-A` option is to enable agent forwarding

SSH to the nodes after you are in bastion

    $ ssh centos@<node IP or DNS>

If you want to sftp to nodes, you can do as normal in jump host

    $ sftp centos@<node IP or DNS>

**SSH Config**

To avoid typing all the commands, you can add below content to  `~/.ssh/confg`

    Host [short host alias name]
        HostName [actual host name or ip]
        ForwardAgent yes
        IdentityFile [private key file path]
        User [user to login]

After that, you can just do `$ ssh [alias name]`

**Debug**
1. Make sure `AllowAgentForwarding` is set to `yes` in `sshd_config`.

2. In jump host run below command, tt should have a value.

        $ echo "$SSH_AUTH_SOCK"


