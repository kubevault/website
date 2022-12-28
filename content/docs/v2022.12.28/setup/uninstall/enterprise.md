---
title: Uninstall KubeVault Enterprise Edition
description: Uninstallation guide for KubeVault Enterprise edition
menu:
  docs_v2022.12.28:
    identifier: uninstall-kubevault-enterprise
    name: Enterprise Edition
    parent: uninstallation-guide
    weight: 20
product_name: kubevault
menu_name: docs_v2022.12.28
section_menu_id: setup
info:
  cli: v0.13.0
  installer: v2022.12.28
  operator: v0.13.0
  unsealer: v0.13.0
  version: v2022.12.28
---

# Uninstall KubeVault Enterprise Edition

To uninstall KubeVault Enterprise edition, run the following command:

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="helm3" role="tabpanel" aria-labelledby="helm3-tab">

## Using Helm 3

In Helm 3, release names are [scoped to a namespace](https://v3.helm.sh/docs/faq/#release-names-are-now-scoped-to-the-namespace). So, provide the namespace you used to install the operator when installing.

```bash
$ helm uninstall kubevault --namespace kubevault
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML (with helm 3)

If you prefer to not use Helm, you can generate YAMLs from KubeVault chart and uninstall using `kubectl`.

```bash
$ helm template kubevault appscode/kubevault-operator --namespace kubevault | kubectl delete -f -
```

</div>
</div>
