---
title: Libosinfo (Part I)
date: 2019-10-16
tags: ["virt", "gnome", "fedora"]
---

This is the first blog post of a series which will cover Libosinfo, what it is,
who uses it, how it is used, how to manage it, and, finally, how to contribute
to it.


### A quick overview

Libosinfo is **the** operating system information database. As a project, it
consists of three different parts, with the goal to provide a single place
containing all the required information about an operating system in order to
provision and manage it in a virtualized environment.

The project allows management applications to:

* Automatically identify for which operating system an ISO image or an
  installation tree is intended to;

* Find the download location of installable ISOs and LiveCDs images;

* Find the location of installation trees;

* Query the minimum, recommended, and maximum CPU / memory / disk resources
  for an operating system;

* Query the hardware supported by an operating system;

* Generate scripts suitable for automating "Server" and "Workstation"
  installations;


### The library (libosinfo)

The library API is written in C, taking advantage of GLib and GObject. Thanks
to GObject Introspection, the API is automatically available in all dynamic
programming languages with bindings for GObject (JavaScript, Perl, Python, and
Ruby). Auto-generated bindings for Vala are also provided.

As part of **libosinfo**, three tools are provided:

* **osinfo-detect**: Used to detect an Operating System from a given ISO or
  installation tree.

* **osinfo-install-script**: Used to generate a "Server" or "Workstation"
  install-script to perform automated installation of an Operating System;

* **osinfo-query**: Used to query information from the database;


### The database (osinfo-db)

The database is written in XML and it can either be consumed via libosinfo APIs
or directly via management applications' own code.

It contains information about the operating systems, devices, installation
scripts, platforms, and datamaps (keyboard and language mappings for Windows
and Linux OSes).


### The database tools (osinfo-db-tools)

These are tools that can be used to manage the database, which is distributed
as a tarball archive.

* **osinfo-db-import**: Used to import an osinfo database archive;

* **osinfo-db-export**: Used to export an osinfo database archive;

* **osinfo-db-validate**: Used to validate the XML files in one of the osinfo
  database locations for compliance with the RNG schema.

* **osinfo-db-path**: Used to report the paths associated with the standard
  database locations;


### The consumers ...

Libosinfo and osinfo-db have management applications as their target audience.
Currently the libosinfo project is consumed by big players in the virtual
machine management environment such as [OpenStack Nova]
(https://docs.openstack.org/nova/latest/), [virt-manager]
(https://virt-manager.org), [GNOME Boxes] (https://wiki.gnome.org/Apps/Boxes),
and [Cockpit Machines]
(https://cockpit-project/guide/latest/feature-virtualmachines).

#### ... a little bit about them ...

* [OpenStack Nova] (https://docs.openstack.org/nova/latest/): An [OpenStack]
  (https://www.openstack.org/) project that provides a way to provision virtual
  machines, baremetal servers, and (limited supported for) system containers.

* [virt-manager] (https://virt-manager.org): An application for managing
  virtual machines through libvirt.

* [GNOME Boxes] (https://wiki.gnome.org/Apps/Boxes): A simple application to
  view, access, and manage remote and virtual systems.

* [Cockpit Machines]
  (https://cockpit-project/guide/latest/feature-virtualmachines): A [Cockpit]
  (https://cockpit-project.org/) extension to manage virtual machines running
  on the host.

#### ... and why they use it

* **Download ISOs**: As libosinfo provides the ISO URLs, management
  applications can offer the user the option to download a specific operating
  system;

    - [ ] OpenStack Nova
    - [ ] virt-manager
    - [x] GNOME Boxes
    - [ ] Cockpit Machines

* **Automatically detect the ISO being used**: As libosinfo can detect the
  operating system of an ISO, management applications can use this info to
  set reasonable default values for resources, to select the hardware
  supported, and to perform unattended installations.

    - [ ] OpenStack Nova
    - [x] virt-manager
    - [x] GNOME Boxes
    - [x] Cockpit Machines

* **Start tree installation**: As libosinfo provides the tree installation
  URLs, management applications can use it to start a network-based
  installation without having to download the whole operating system ISO;

    - [ ] OpenStack Nova
    - [x] virt-manager (via virt-install)
    - [ ] GNOME Boxes
    - [x] Cockpit Machines

* **Set reasonable default values for RAM, CPU, and disk resources**: As
  libosinfo knows the values that are recommended by the operating system's
  vendors, management applications can rely on that when setting the default
  resources for an installation.

    - [ ] OpenStack Nova
    - [x] virt-manager
    - [x] GNOME Boxes
    - [x] Cockpit Machines

* **Automatically set the hardware supported**: As libosinfo provides the list
  of hardware supported by an operating system, management applications can
  choose the best defaults based on this information, without taking the risk
  of ending up with a non-bootable guest.

    - [x] OpenStack Nova
    - [x] virt-manager
    - [x] GNOME Boxes
    - [x] Cockpit Machines (via virt-install)

* **Unattended install**: as libosinfo provides unattended installations
  scripts for CentOS, Debian, Fedora, Fedora Silverblue, Microsoft Windows, 
  OpenSUSE, Red Hat Enterprise Linux, and Ubuntu, management applications can
  perform unattended installations for both "Workstation" and "Server"
  profiles.

    - [ ] OpenStack Nova
    - [x] virt-manager (via virt-install)
    - [x] GNOME Boxes (only supports "Workstation" installations)
    - [ ] Cockpit Machines (this is still a [work-in-progress]
          (https://github.com/cockpit-project/cockpit/pull/12716))

### What's next?

The next blog post will provide a "demo" of an unattended installation using
both GNOME Boxes and virt-install and, based on that, explain how libosinfo is
internally used by these projects.

By doing that, we'll both cover how libosinfo can be used and also demonstrate
how it can ease the usage of those management applications.
