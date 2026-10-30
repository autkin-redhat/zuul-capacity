# zuul-capacity

This is a custom prometheus exporter to provide the available capacity as a metric:

```ShellSession
$ uv run ./zuul-capacity.py --help
usage: zuul-capacity.py [-h] [--nodepool FILE] [--port PORT]
                        [--kubevirt-kubeconfig FILE]
                        [--kubevirt-context KUBEVIRT_CONTEXT]

options:
  -h, --help            show this help message and exit
  --nodepool FILE       The list of providers
  --port PORT           The prometheus scrap target listening port
  --kubevirt-kubeconfig FILE
                        Kubeconfig for a KubeVirt-backed provider
  --kubevirt-context KUBEVIRT_CONTEXT
                        Kubeconfig context to use for the KubeVirt provider,
                        also used as the provider name
```

To run this service, you must provide a nodepool configuration and a clouds.yaml.

A KubeVirt-backed provider (e.g. a nodepool driver provider backed by
[prekv](https://gitlab.com/softwarefactory-project/prekv)) isn't discovered
from the nodepool configuration, since its resources (CPU, memory, disk) are
not exposed through the API. Pass `--kubevirt-kubeconfig` to
report it instead, by querying the KubeVirt `VirtualMachine` resources in the
kubeconfig context's namespace directly; the context name is used as the
`cloud` label, and only VMs carrying a non-empty `prekv.io/nodepool_provider_name`
annotation (i.e. nodepool-managed VMs) are counted.

## Changelog

### 0.6.0

- Added direct KubeVirt provider support via `--kubevirt-kubeconfig`, using the
  kubeconfig context name as the provider name

### 0.5.0

- Added a zuul_instances_disk metric

### 0.4.0

- Initial release
