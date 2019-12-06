---
title: Uninstall Vault CSI Driver
menu:
  docs_v0.3.0:
    identifier: uninstall-csi-driver
    name: Uninstall
    parent: csi-driver-setup
    weight: 15
menu_name: docs_v0.3.0
section_menu_id: setup
info:
  version: v0.3.0
---

# Uninstall Vault CSI Driver

If you installed csi driver using YAML then run:

```console
$ curl -fsSL https://github.com/kubevault/csi-driver/raw/{{< param "info.version" >}}/hack/deploy/install.sh \
    | bash -s -- --uninstall [--namespace=NAMESPACE]

```

The above command will leave the csidriver crd objects as-is. If you wish to nuke all csidriver crd objects, also pass the `--purge` flag.

If you used HELM to install Vault CSI driver, then run following command

```console
helm del --purge <name>
```
