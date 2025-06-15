---
title: Deploy to a Folder
layout: default
parent: Deployment
nav_order: 40
---

# Deploy to a Folder

## Synopsis

Maven can out of the box manage artifacts in a folder and deploy to it.

## Purpose

* Deploying artifacts to a folder allows publishing resolveable artifacts using a plain Web server.
* This can be a lightweight alternative to a fully fledged repository system by minimizing the amount of software that needs to be maintained.
* File systems such as btrfs and ZFS can be leveraged to (a) reduce disk space usage using built-in compression and (b) to perform blazing fast backups by transferring only the blocks changed in the file system (no need for a full scan of the repository).

## Approach

Use the `mvn deploy` with the `-DaltDeploymentRepository` option to specify where to deploy. Note, that the path after `file:` may be a relative one!

```bash
mvn deploy -DaltDeploymentRepository=snapshot-repo::default::file:/tmp/my-local-snapshots-dir
```

## Notes

* Using `sshfs` a remote folder can be mounted locally and deployed to.

```bash
sshfs user@remote-server:/some/maven-repo

mvn deploy -DaltDeploymentRepository=snapshot-repo::default::file:./maven-repo
```

## References

* [https://stackoverflow.com/questions/12549742/maven-deploy-to-relative-directory-path](https://stackoverflow.com/questions/12549742/maven-deploy-to-relative-directory-path)

