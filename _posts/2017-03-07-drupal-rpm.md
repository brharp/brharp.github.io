---
title: Drupal RPM
layout: post
---

## Packaging a Module

Log in to the dev server (13) as wsadmin. The following example shows how to package the media module version 7.x-2.0-rc3.

```
$ cd rpmbuild
$ ./package-drupal-module.sh media 7.x-2.0-rc3
```

If you need to customize the build (e.g. by applying a patch),
edit `SPECS/drupal-media.spec.in` and run `package-drupal-module.sh` again.

## Packaging Drupal

```
$ ./package-drupal.sh version
```

## Updating the Repo

Run `./update-repo.sh` to update the repository metadata and publish the repository.

## Prepare the Server

Create the following file: `/etc/yum.repos.d/drupal.repo`

```
# /etc/yum.repos.d/drupal.repo
[drupal]
name=Drupal
baseurl=http://www.uoguelph.ca/ccs/repos/drupal/drupal-$releasever/
enabled=0
skip_if_unavailable=1
gpgcheck=0
```

## Install Drupal

```
# yum --enablerepo=drupal install drupal
```

## Update Drupal

```
# yum --enablerepo=drupal update drupal
```

## Install a Module

```
# yum --enablerepo=drupal install module
```

## Update a Module

```
# yum --enablerepo=drupal update module
```

## Directory Layout

Drupal files are distributed according to the Linux Filesystem Hierarchy standard. Configuration files (sites) are in `/etc/drupal`. Files are in `/var/lib/drupal`. PHP code is in `/usr/share/drupal`.

```
/etc/drupal
    /sites
        /all -> /usr/share/drupal
        /default
            /files -> /var/lib/drupal/files
/usr/share/drupal
    /core
        /sites -> /etc/drupal/sites
    /modules   (aka sites/all/modules)
    /themes    (aka sites/all/themes)
    /libraries (aka sites/all/libraries)
/var/lib/drupal/files
```
