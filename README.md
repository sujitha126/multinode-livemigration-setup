Devstack-Environment
====================

A single- or multi-node DevStack deployment driven by Vagrant and Ansible.

This allows you to bring up a complete single- or multi-node DevStack
deployment with just a single command.

You can deploy either a virtual environment or Vagrant, or use the
Ansible playbooks on their own to deploy on real hardware.

Note: Forked from https://github.com/CiscoSystems/devstack-environment and
worked on getting on necessary changes to get live migration setup work on multinode.

Prerequisites (for hardware deployments)
========================================
You need to have installedi on Ansible-server:

    - Ansible

On Ubuntu based systems, you should be able to do this:

    $ apt-get ansible

If you want to make sure you have the latest version of Ansible (highly
recommended), and you are still running an older version of Ubuntu (such
as 12.04, for example), you could instead do:

    $ apt-get install python-pip
    $ pip install ansible


On controller and compute node, install python-apt package.

    $ apt-get install python-apt

Provide password less ssh access to controller and compute nodes from ansible-server

On Ansible server:

    $ ssh-keygen
    $ ssh-copy-id ansible-controller-ip
    $ ssh-copy-id ansible-compute-ip


Instructions (for hardware deployments)
=======================================

    0. Have some hardware machines available with fresh installs of Ubuntu 14.04.
       Please make sure that you have a user account on each of those machines,
       which allows password-less sudo. If you have an SSH key for password-less
       login to those accounts, that's great as well, but is not mandatory.

    1. Clone this repository:

        $ git clone https://github.com/sujitha126/multinode-livemigration-setup.git

    2. Change into the directory:

        $ cd multinode-livemigration-setup

    3. Edit the 'hosts' file to change the IP addresses of your actual
       machines. You don't have to use a cache machine, so the apt-pip-cache
       machine may be removed.

    4. Open the 'vars/extra_vars.yml' file. Change CACHE.pkg_cache to none in vars/extra_vars.yml.

    5. Start the Ansible playbooks like so:

        $ ansible-playbook -i hosts -e "@vars/extra_vars.yml" site.yml

    6. Restart nova services on each node and try live migration.
