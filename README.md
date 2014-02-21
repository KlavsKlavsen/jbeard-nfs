#nfs

####Table of Contents

1. [Overview](#overview)
2. [Module Description - What the module does and why it is useful](#module-description)
3. [Setup - The basics of getting started with nfs](#setup)
    * [What nfs affects](#what-nfs-affects)
    * [Setup requirements](#setup-requirements)
    * [Beginning with nfs](#beginning-with-nfs)
4. [Usage - Configuration options and additional functionality](#usage)
5. [Reference - An under-the-hood peek at what the module is doing and how](#reference)
6. [Limitations - OS compatibility, etc.](#limitations)
7. [Development - Guide for contributing to the module](#development)

##Overview
This is a module designed for controlling the [Network File System][wikipedia] server and client daemons.

##Module Description
This module installs and configures the [Network File System][wikipedia] servers and clients.

##Setup

###What nfs affects
The nfs module will install the appropriate packages for RedHat and Debian distributions.
It also controls execution of the appropriate daemon services.

###Setup requirements
This module requires the [puppetlabs/stdlib][stdlib] module and the [jbeard/portmap][portmap] modules.

###Beginning with nfs
The nfs module is broken into two main components: `nfs::client` for those hosts that only wish to
consume NFS exports from another server, and `nfs::server` for those hosts that wish also to export
filesystems via NFS.  Importing the `nfs` class directly will have no affect.  At this time, inclusion
of the `nfs::server` class automatically includes the `nfs::client` class as well.

Furthermore, for hosts that import the `nfs::server` class, the `nfs::export` type becomes available.
This type exposes the `/etc/exports` file with a puppet-like interface.

##Usage

###NFS Client
To enable a host to act as an NFS client, simply include the `nfs::client` class in the manifest.

    include nfs::client

The resource may optionally be specified as

    class { 'nfs::client':
        ensure => installed,
    }

###NFS Server
To enable a host to act as an NFS server, include the `nfs::server` class in the manifest and,
optionally, some `nfs::export` resources.

    class { 'nfs::server':
        package => latest,
        service => running,
        enable  => true,
    }
    
    nfs::export { '/srv/shared':
        options => [ 'rw', 'async' ],
        clients => [ "${::network_eth0}/${netmask_eth0}" ],
    }

##Reference
_TODO_ List all the classes and organization

##Limitations
The nfs module is currently only supported on RedHat Enterprise Linux 5/6, CentOS 5/6, and Debian (Client only).

Furthermore, use of the `nfs::server` class dictates that all exports be defined with the `nfs::export` resource type.

##Development
_TODO_ Document development practices

[wikipedia]: http://en.wikipedia.org/wiki/Network_File_System "Network File System - Wikipedia, the free encyclopedia"
[portmap]: http://forge.puppetlabs.com/jbeard/portmap "jbeard/portmap - Puppet Forge"
[stdlib]: http://forge.puppetlabs.com/puppetlabs/stdlib "puppetlabs/stdlib - Puppet Forge"

