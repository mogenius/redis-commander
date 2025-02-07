# redis-commander

A Helm chart for redis-commander

![Version: 0.1.2](https://img.shields.io/badge/Version-0.1.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: latest](https://img.shields.io/badge/AppVersion-latest-informational?style=flat-square)

**Install**

Install using this repo after local git checkout itself with setting redis server host value
to `redis`

```sh
cd <git-repo>
helm -n myspace install redis-web-ui ./k8s/helm-chart/redis-commander --set redis.host=redis
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling | object | `{"enabled":false,"maxReplicas":1,"minReplicas":1,"targetCPUUtilizationPercentage":80}` | Autoscaling configuration for k8s deployment |
| env | list | `[]` | Extra env vars for the main container redis-commander in array structure ([{name: ... , value: ...}, {name: ... , value: ...}]). |
| fullnameOverride | string | `""` |  |
| httpAuth.password | string | `""` | Specify http basic password for the web ui |
| httpAuth.username | string | `""` | Specify http basic username and password to protect access to redis commander web ui |
| image.apparmorProfile | string | `"runtime/default"` | Enable AppArmor per default when available on k8s host, change to "unconfined" to disable -- either AppArmor or SecComp may be enabled by the container runtime |
| image.pullPolicy | string | `"Always"` | Deployment pull policy, either "Always" or "IfNotPresent" |
| image.repository | string | `"rediscommander/redis-commander"` | Docker image for deployment |
| image.seccompProfile | string | `"runtime/default"` | Enable SecComp profile when used by cluster, change to "unconfined" to disable -- either AppArmor or SecComp may be enabled by the container runtime |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Optional image pull secrets for private docker registries |
| ingress.annotations | object | `{}` | Add additional annotations for the ingess spec Example:   'kubernetes.io/ingress.class: nginx' or 'kubernetes.io/tls-acme: "true"' |
| ingress.enabled | bool | `false` | Enable Ingress for the service |
| ingress.hosts[0] | object | `{"host":"chart-example.local","paths":[]}` | Host name to use for the ingress definition |
| ingress.hosts[0].paths | list | `[]` | Optional list of paths within the given host for path-based routing |
| ingress.tls | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| redis.host | string | `"redis-master"` | Specifies a single Redis host |
| redis.hosts | string | `""` | Alternative: Specifies multiple redis endpoints <label:host:port>,... instead of one in "redis.host" Example: "local:localhost:6379,myredis:10.10.20.30" |
| redis.password | string | `""` | Specifies redis password |
| replicaCount | int | `1` |  |
| resources | object | `{}` | We usually recommend not to specify default resources and to leave this as a conscious choice for the user. This also increases chances charts run on environments with little resources, such as Minikube. If you do want to specify resources, uncomment the following lines, adjust them as necessary, and remove the curly braces after 'resources:'. |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":false,"runAsNonRoot":true}` | Configuration of the linux security context for the docker image. This restricts the rights of the running docker image as far as possible. "readOnlyRootFilesystem" must be set to false to auto-generate a config file with multiple redis hosts or sentinel hosts |
| service.port | int | `80` | External port where service is available |
| service.type | string | `"ClusterIP"` | Type of k8s service to export |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `false` | Specifies whether a service account should be created When no service account is created the account credentials of the default account are also not automatically mounted into the pod (automountServiceAccountToken: false), tokens only mounted when service account is used but Redis-Commander itself does not use the k8s api server token |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` |  |

**Example**

Another alternative is the usage of the helm repo hosted at the github pages site.
```sh
# add repo
helm repo add redis-commander https://joeferner.github.io/redis-commander/

# custom values
cat > myvalues.yaml <<EOF
redis.host: redis-master
# env:
# - name: FOLDING_CHAR
#   value: "/"
ingress:
  enabled: true
  annotations:
    kubernetes.io/ingress.class: nginx
  hosts:
    - host: redis-ui.example.com
      paths: ["/"]
EOF

# install helm chart with the custom values
helm install \
  redis-web-ui \
  redis-commander/redis-commander \
  -f myvalues.yaml

```

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.5.0](https://github.com/norwoodj/helm-docs/releases/v1.5.0)
