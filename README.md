Ansible Role: metareleases
==========================

The aim of this role is to generate the meta-release files that Ubuntu Release Upgrader uses to determine which Ubuntu distributions are supported and how to upgrade between them.

What this role doesn't do however is to edit `/etc/update-manager/meta-release` on the clients to use the generated data.

Role Variables
--------------

The path on the remote you would like to populate with your meta-release files.

    metareleases_path: /var/www

Ownership of files and directories can be set.

    metareleases_owner: www-data
    metareleases_group: www-data

The list of releases you would like to include in your meta-release files. We have made this a list of dicts so that we can override parts from `metareleases_info` in the future. Initially it's possible to override the support status of a release.

We only support releases also available in `metareleases_info`

By default we will generate empty files because of an empty list. This example is not showing the default.

    metareleases:
      - name: focal
      - name: groovy
        support: true

`metareleases_info` are editable, but hopefully we will be able to keep it up to date enough that this variable will rarely need touched. The example here only contains a small set of releases, but the actual variable will include all releases since Ubuntu 14.04 (Trusty Tahr).

The `support` variable will be converted to boolean when generating the files, with strings always becoming false. The value `devel` is a special case since it's used to generate the `*-development` files.

    metareleases_info:
      - name: Focal Fossa
        version: 20.04.1 LTS
        date: Thu, 23 April 2020 20:04:00 UTC
        support: true
      - name: Groovy Gorilla
        version: '20.10'
        date: Thu, 22 October 2020 20:22:00 UTC
        support: devel

The variable for which files to generate. You can not edit this variable outside the role.

    metareleases_files:
      - meta-release
      - meta-release-development
      - meta-release-proposed
      - meta-release-lts
      - meta-release-lts-development
      - meta-release-lts-proposed


Example Playbook
----------------

    - hosts: metareleases
      vars:
        metareleases:
          - name: focal
          - name: groovy
      roles:
        - vcc-caeit.metareleases

License
-------

GPLv2

Author Information
------------------

This role was created in 2020 by Nafallo Bjälevik, whilst doing consultancy work for [Volvo Cars Corporation](http://www.volvocars.com/).
