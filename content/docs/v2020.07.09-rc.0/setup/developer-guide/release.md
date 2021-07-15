---
title: Release | Vault operator
menu:
  docs_v2020.07.09-rc.0:
    identifier: release
    name: Release
    parent: developer-guide
    weight: 15
menu_name: docs_v2020.07.09-rc.0
section_menu_id: setup
info:
  cli: v0.4.0-rc.0
  csi: v0.4.0-rc.0
  installer: v2021.07.14-rc.0
  operator: v0.4.0-rc.0
  unsealer: v0.4.0-rc.0
  version: v2020.07.09-rc.0
---

# Release Process

The following steps must be done from a Linux x64 bit machine.

- Do a global replacement of tags so that docs point to the next release.
- Push changes to the `release-x` branch and apply new tag.
- Push all the changes to remote repo.
- Build and push vault docker image:
```console
$ cd ~/go/src/github.com/appscode/vault
./hack/docker/setup.sh; env APPSCODE_ENV=prod ./hack/docker/setup.sh release
```

- Now, update the release notes in Github. See previous release notes to get an idea what to include there.
