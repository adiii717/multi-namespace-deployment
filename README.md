## Multi-Namespace Deployment
This helm-chart template will help you to deploy a multi-namespace deployment/cronjob etc.

## Static vs Dynamic namespace

To deploy, you can either specify the static namespace through values file

*Static NameSpace:*

```yaml
namespaces:
  - namespace-1
  - namespace-2
```

*Dynamic NameSpace:*

You can all pass dynamic namespaces all or certain namespaces, for example all namespaces

```shell
helm template  helm-chart --set "namespaces={$(k get ns | awk 'BEGIN{ORS=","} { if (NR>1) print $1}')}"
helm install my-cronjob  helm-chart --set "namespaces={$(k get ns | awk 'BEGIN{ORS=","} { if (NR>1) print $1}')}"
```

If you to deploy certain namespaces, you can pass the condition
For example deploy the following chart to all namespaces that contain `develop`

```shell
#Dry test:
helm template cronjob helm-chart --set "namespaces={$(k get ns | awk 'BEGIN{ORS=","} { if (NR>1 && $1 ~ "develop") print $1}')}"
#Deploy:
helm install my-cronjob-release helm-chart --set "namespaces={$(k get ns | awk 'BEGIN{ORS=","} { if (NR>1 && $1 ~ "develop") print $1}')}"
```
