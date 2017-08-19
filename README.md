Ansible Role: Aerospike
=========

[![Ansible Role](https://img.shields.io/ansible/role/19856.svg)](https://galaxy.ansible.com/mplachter/aerospike/) [![Build Status](https://travis-ci.org/mplachter/ansible-role-aerospike.svg?branch=master)](https://travis-ci.org/mplachter/ansible-role-aerospike)

Ansible role to install and configure [Aerospike](http://www.aerospike.com/) on CentOS (6/7) and Debian (Jessie\Wheezy) Linux.

Requirements
------------

* Ansible 2.3+
* CentOS 6+
* Debian Wheezy\Jessie

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

    aerospike_boot_enabled: true

Controls enabling Aerospike to start on boot.

    aerospike_version: 3.14.1.1
    aerospike_tools_version: 3.13.0.1

Controls the version of Aerospike server and tools, respectively.
See [Aerospike releases](http://www.aerospike.com/download/server/notes.html) for complete list.

    aerospike_source_directory: /usr/local/src

Controls the expected name of the unarchived Aerospike tools package.

    aerospike_log_location: /var/log/aerospike

Controls the expected location of Aerospike logs. (currently only supports single log **aerospike.log** at location will all contexts)[Aerospike Logging Guide Docs](http://www.aerospike.com/logging-guide/)

    aerospike_cluster_size: 1

Controls the expected number of nodes in the Aerospike server cluster.

Optional variables
------------------

The following variables can be set to overwrite the default values (default values listed below).

    aerospike_proto_fd_max: 15000

Maximum number of open file descriptors opened on behalf of client connections. [proto-fd-max Docs](http://www.aerospike.com/docs/reference/configuration#proto-fd-max)

    aerospike_proto_fd_idle_ms: 60000

Time in milliseconds to wait before reaping connections. [proto-fd-idle-ms Docs](http://www.aerospike.com/docs/reference/configuration#proto-fd-idle-ms)

    aerospike_access_address: "{{ ansible_default_ipv4.address }}"

An access address is an IP address that is announced to clients and used by clients for connecting to the cluster. [access-address Docs](http://www.aerospike.com/docs/reference/configuration#access-address)

Using a managed configuration file
----------------------------------

**All defaults below apply to a managed configuration file.**

    aerospike_namespaces:
      - name: default

Controls namespace configuration of the Aerospike server.
See [Aerospike namespace configuration](http://www.aerospike.com/docs/operations/configure/namespace/) for details.

You can list multiple namespaces with file, memory, or device storage engines.

    aerospike_namespaces:
      - name: device_objects
        memory_size: 8
        storage_engine:
          devices:
            - /dev/sdb
            - /dev/dsc
          scheduler_mode: noop
          write_block_size: 128K
      - name: file_objects
        storage_engine:
          files:
            - /opt/aerospike/data/1
            - /opt/aerospike/data/2
          data_in_memory: true
       - name: memory_objects

Above is an example of configuring 3 namespaces using attached devices, files, and memory. [Aerospike Storage Engines Docs](http://www.aerospike.com/docs/operations/configure/namespace/storage#comparing-storage-engines)

    aerospike_service_threads: 4

Controls the number of threads receiving client requests on the network interface.
[service-threads Docs](http://www.aerospike.com/docs/reference/configuration/#service-threads)

    aerospike_transaction_queues: 4

Controls the number of transaction queues managing client requests.
Service threads will dispatch transactions into those queues.
[transaction-queues Docs](http://www.aerospike.com/docs/reference/configuration/#transaction-queues)

    aerospike_transaction_threads: 4

Controls the number of threads per transaction queue. 
Those threads will consume the requests from the the transaction queues.
[transaction-threads Docs](http://www.aerospike.com/docs/reference/configuration/#transaction-threads-per-queue)

    aerospike_mesh_seed_addresses:
      - 127.0.0.1

Controls the list of mesh addresses of all nodes in the heartbeat cluster. Applies only when the node is mesh
[mesh-seed-address-port Docs](http://www.aerospike.com/docs/reference/configuration/#mesh-seed-address-port)

    aerospike_multicast_group: 239.1.99.2
    aerospike_multicast_port: 9918
    aerospike_multicast_address: 10.100.10.101

You may also use the multicast heartbeat cluster. If multicast group is defined it will take presidency over `aerospike_mesh_seed_addresses`
[multicast-group Docs](http://www.aerospike.com/docs/reference/configuration#multicast-group) 
[multicast heartbeat Docs](http://www.aerospike.com/docs/operations/configure/network/heartbeat#multicast-heartbeat)

Role Dependencies
------------

None.

Example Playbook
----------------

    ---
    - hosts: all
      roles:
         - mplachter.aerospike

Testing
-------

This role is tested through [Molecule](http://molecule.readthedocs.io/en/stable-1.25/configuration.html)

License
-------

MIT

Author Information
------------------

Matt Plachter
