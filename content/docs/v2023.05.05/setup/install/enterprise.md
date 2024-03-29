---
title: Install KubeVault Enterprise Edition
description: Installation guide for KubeVault Enterprise edition
menu:
  docs_v2023.05.05:
    identifier: install-kubevault-enterprise
    name: Enterprise Edition
    parent: installation-guide
    weight: 20
product_name: kubevault
menu_name: docs_v2023.05.05
section_menu_id: setup
info:
  cli: v0.15.0
  installer: v2023.05.05
  operator: v0.15.0
  unsealer: v0.15.0
  version: v2023.05.05
---

# Install KubeVault Enterprise Edition

KubeVault comes in 2 editions: `Community Edition` and `Enterprise Edition`. `Community Edition` only manages KubeVault custom resources in the `default` Kubernetes namespace. `Enterprise Edition` can be used to manage KubeVault custom resources in any Kubernetes namespace. A full features comparison between the KubeVault Community edition and Enterprise edition can be found [here](https://kubevault.com/pricing/).

If you are willing to try KubeVault Enterprise Edition, you can grab a **30 days trial** license from [here](https://license-issuer.appscode.com/?p=kubevault-enterprise).

## Get a Trial License

In this section, we are going to show you how you can get a **30 days trial** license for KubeVault Enterprise edition. You can get a license for your Kubernetes cluster by going through the following steps:

- At first, go to [AppsCode License Server](https://license-issuer.appscode.com/?p=kubevault-enterprise) and fill up the form. It will ask for your Name, Email, the product you want to install, and your cluster ID (UID of the `kube-system` namespace).
- Provide your name and email address. **You must provide your work email address**.
- Then, select `KubeVault Enterprise Edition` in the product field.
- Now, provide your cluster ID. You can get your cluster ID easily by running the following command:

```bash
kubectl get ns kube-system -o=jsonpath='{.metadata.uid}'
```

- Then, you have to agree with the terms and conditions. We recommend reading it before checking the box.
- Now, you can submit the form. After you submit the form, the AppsCode License server will send an email to the provided email address with a link to your license file.
- Navigate to the provided link and save the license into a file. Here, we save the license to a `license.txt` file.

Here is a screenshot of the license form.

<figure align="center">
  <img alt="KubeVault Backend Overview" src="/docs/v2023.05.05/images/setup/enterprise_license_form.png">
  <figcaption align="center">Fig: KubeVault License Form</figcaption>
</figure>

You can create licenses for as many clusters as you want. You can upgrade your license any time without re-installing KubeVault by following the upgrading guide from [here](/docs/v2023.05.05/setup/upgrade/#updating-license).

> KubeVault licensing process has been designed to work with CI/CD workflow. You can automatically obtain a license from your CI/CD pipeline by following the guide from [here](https://github.com/appscode/offline-license-server#offline-license-server).

## Get an Enterprise License

If you are interested in purchasing Enterprise license, please contact us via sales@appscode.com for further discussion. You can also set up a meeting via our [calendly link](https://calendly.com/appscode/30min).

If you are willing to purchasing Enterprise license but need more time to test in your dev cluster, feel free to contact sales@appscode.com. We will be happy to extend your trial period.

## Install

To activate the Enterprise features, you need to install both KubeVault Community operator and Enterprise operator chart. These operators can be installed as a Helm chart or simply as Kubernetes manifests. If you have already installed the Community operator, only install the Enterprise operator (step 4 in the following secttion).

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3 (Recommended)</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="helm3" role="tabpanel" aria-labelledby="helm3-tab">

## Using Helm 3

KubeVault can be installed via [Helm](https://helm.sh/) using the [chart](https://github.com/kubevault/installer/tree/{{< param "info.installer" >}}/charts/kubevault) from [AppsCode Charts Repository](https://github.com/appscode/charts). To install, follow the steps below:

```bash
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update

$ helm search repo appscode/kubevault
NAME                        CHART VERSION APP VERSION DESCRIPTION                                       
appscode/kubevault          {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault by AppsCode - HashiCorp Vault operato...
appscode/kubevault-catalog  {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault Catalog by AppsCode - Catalog for Kub...
appscode/kubevault-crds     {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault Custom Resource Definitions             
appscode/kubevault-operator {{< param "info.operator" >}}        {{< param "info.operator" >}}      KubeVault Operator by AppsCode - HashiCorp Vaul...

# Install KubeVault Enterprise operator chart
$ helm install kubevault appscode/kubevault \
    --version {{< param "info.version" >}} \
    --namespace kubevault --create-namespace \
    --set-file global.license=/path/to/the/license.txt
```

To see the detailed configuration options, visit [here](https://github.com/kubevault/installer/tree/{{< param "info.installer" >}}/charts/kubevault).

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML

If you prefer to not use Helm, you can generate YAMLs from KubeVault chart and deploy using `kubectl`. Here we are going to show the procedure using Helm 3.

```bash
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update

$ helm search repo appscode/kubevault
NAME                        CHART VERSION APP VERSION DESCRIPTION                                       
appscode/kubevault          {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault by AppsCode - HashiCorp Vault operato...
appscode/kubevault-catalog  {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault Catalog by AppsCode - Catalog for Kub...
appscode/kubevault-crds     {{< param "info.version" >}}   {{< param "info.version" >}} KubeVault Custom Resource Definitions             
appscode/kubevault-operator {{< param "info.operator" >}}        {{< param "info.operator" >}}      KubeVault Operator by AppsCode - HashiCorp Vaul...

# Install KubeVault Enterprise operator chart
$ helm template kubevault appscode/kubevault \
    --version {{< param "info.version" >}} \
    --namespace kubevault --create-namespace \
    --set-file global.license=/path/to/the/license.txt \
    --set global.skipCleaner=true | kubectl apply -f -
```

To see the detailed configuration options, visit [here](https://github.com/kubevault/installer/tree/{{< param "info.installer" >}}/charts/kubevault).

</div>
</div>

## Verify installation

To check if KubeVault operator pods have started, run the following command:

```bash
$ watch kubectl get pods --all-namespaces -l "app.kubernetes.io/instance=kubevault"

NAMESPACE   NAME                                            READY   STATUS    RESTARTS   AGE
kubevault   kubevault-kubevault-operator-5d5cc4c7c9-mj5d5   1/1     Running   0          2m18s
```

Once the operator pod is running, you can cancel the above command by typing `Ctrl+C`.

Now, to confirm CRD groups have been registered by the operator, run the following command:

```bash
$ kubectl get crd -l app.kubernetes.io/name=kubevault
```

Now, you are ready to [create your first database](/docs/v2023.05.05/guides/README) using KubeVault.
